---
title: Android
layout: 2017/sheet
---

Activities & Fragments
---------
{: .-three-column}

### Activity Lifecycle

![Activity Lifecycle](https://developer.android.com/guide/components/images/activity_lifecycle.png)

[Documentation](https://developer.android.com/guide/components/activities/activity-lifecycle)


Persistant Storage
------------------

### Shared Preferences

```kotlin
val prefs = context.getSharedPreferences("CHEATSHEET_PREFS", Context.MODE_PRIVATE)
prefs.edit().putBoolean("CHEATSHEET_ARG", true).apply()

val isCheatsheet = prefs.getBoolean("CHEATSHEET_ARGS", false)
```

[Documentation](https://developer.android.com/reference/android/content/SharedPreferences)

### Room

```kotlin
// Create Entity
@Entity
class Task(
    @PrimaryKey(autoGenerate = true) var id: Int = 0,
    var description: String = "",
    var completed: Boolean = false
)

// Create DAO
@Dao
interface TaskDAO {
    @Query("SELECT * FROM task")
    fun getAll(): LiveData<List<Task>>
}

// Create Database
@Database(entities = arrayOf(Task::class), version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun taskDao(): TaskDAO

    companion object {
        private var INSTANCE: AppDatabase? = null
            private set

        fun getInMemoryDatabase(context: Context): AppDatabase {
            if (INSTANCE == null) {
                INSTANCE = Room.databaseBuilder(
                        context, 
                        AppDatabase::class.java, 
                        "todo-list:"
                    )
                    .build()
            }

            return INSTANCE!!
        }
    }
}

// Query
val tasksLiveData = AppDatabase.getInMemoryDatabase(context).taskDao().getAll()
```

[Documentation](https://developer.android.com/topic/libraries/architecture/room)

[Android Essence Tutorial](https://androidessence.com/android/getting-started-with-room-persistence-library/)