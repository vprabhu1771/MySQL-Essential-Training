To convert the above MySQL logic into a Laravel implementation, we can use the following steps:

1. **Migrations**:
   Create migrations for `students` and `marksheet` tables.

2. **Models**:
   Create models for `Student` and `Marksheet`.

3. **Eloquent Relationships**:
   Define relationships between `Student` and `Marksheet`.

4. **Business Logic**:
   Implement the logic to calculate `status` (Pass/Fail) in the `Marksheet` model using Laravel's Eloquent events.

---

### Laravel Implementation

#### 1. Migrations

```php
// database/migrations/create_students_table.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateStudentsTable extends Migration
{
    public function up()
    {
        Schema::create('students', function (Blueprint $table) {
            $table->id('student_id');
            $table->string('student_name');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('students');
    }
}

// database/migrations/create_marksheet_table.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateMarksheetTable extends Migration
{
    public function up()
    {
        Schema::create('marksheet', function (Blueprint $table) {
            $table->id('marksheet_id');
            $table->foreignId('student_id')->constrained('students', 'student_id')->onDelete('cascade');
            $table->string('subject_name');
            $table->integer('subject_mark');
            $table->string('status');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('marksheet');
    }
}
```

---

#### 2. Models

```php
// app/Models/Student.php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Student extends Model
{
    use HasFactory;

    protected $primaryKey = 'student_id';
    protected $fillable = ['student_name'];

    public function marksheets()
    {
        return $this->hasMany(Marksheet::class, 'student_id');
    }
}

// app/Models/Marksheet.php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Marksheet extends Model
{
    use HasFactory;

    protected $primaryKey = 'marksheet_id';
    protected $fillable = ['student_id', 'subject_name', 'subject_mark', 'status'];

    public function student()
    {
        return $this->belongsTo(Student::class, 'student_id');
    }

    // Automatically calculate status on saving marks
    protected static function boot()
    {
        parent::boot();

        static::saving(function ($marksheet) {
            $marksheet->status = $marksheet->subject_mark >= 35 ? 'Pass' : 'Fail';
        });
    }
}
```

---

#### 3. Controller

```php
// app/Http/Controllers/MarksheetController.php
namespace App\Http\Controllers;

use App\Models\Marksheet;
use App\Models\Student;
use Illuminate\Http\Request;

class MarksheetController extends Controller
{
    public function addMark(Request $request)
    {
        $request->validate([
            'student_id' => 'required|exists:students,student_id',
            'subject_name' => 'required|string',
            'subject_mark' => 'required|integer|min:0',
        ]);

        $marksheet = Marksheet::create($request->all());
        return response()->json(['message' => 'Mark added successfully', 'data' => $marksheet]);
    }

    public function updateMark(Request $request, $marksheet_id)
    {
        $request->validate([
            'subject_mark' => 'required|integer|min:0',
        ]);

        $marksheet = Marksheet::findOrFail($marksheet_id);
        $marksheet->update(['subject_mark' => $request->subject_mark]);

        return response()->json(['message' => 'Mark updated successfully', 'data' => $marksheet]);
    }

    public function viewMarks($student_id)
    {
        $student = Student::with('marksheets')->findOrFail($student_id);
        return response()->json($student);
    }
}
```

---

#### 4. Routes

```php
// routes/web.php
use App\Http\Controllers\MarksheetController;

Route::post('/marks', [MarksheetController::class, 'addMark']);
Route::put('/marks/{marksheet_id}', [MarksheetController::class, 'updateMark']);
Route::get('/marks/{student_id}', [MarksheetController::class, 'viewMarks']);
```

---

#### 5. Example Usage

1. **Add a Student**:
   Use Tinker or Seeder to create students:
   ```php
   Student::create(['student_name' => 'John Doe']);
   Student::create(['student_name' => 'Jane Smith']);
   ```

2. **Add Marks**:
   POST request to `/marks`:
   ```json
   {
       "student_id": 1,
       "subject_name": "Math",
       "subject_mark": 40
   }
   ```

3. **Update Marks**:
   PUT request to `/marks/{marksheet_id}`:
   ```json
   {
       "subject_mark": 50
   }
   ```

4. **View Marks**:
   GET request to `/marks/{student_id}`.

---

### Summary

This Laravel implementation follows best practices using migrations, models, Eloquent events, and a RESTful controller to manage the marksheet system. The `status` is automatically calculated based on the `subject_mark` during save operations.