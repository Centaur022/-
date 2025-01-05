import json
import datetime

class Task:
    def __init__(self, name, description, deadline, priority):
        self.name = name
        self.description = description
        self.deadline = deadline
        self.priority = priority
        self.completed = False

    def mark_completed(self):
        self.completed = True

    def to_dict(self):
        return {
            "name": self.name,
            "description": self.description,
            "deadline": self.deadline,
            "priority": self.priority,
            "completed": self.completed
        }

class TodoList:
    def __init__(self):
        self.tasks = []

    def add_task(self, name, description, deadline, priority):
        task = Task(name, description, deadline, priority)
        self.tasks.append(task)

    def update_task(self, index, name, description, deadline, priority):
        if 0 <= index < len(self.tasks):
            task = self.tasks[index]
            task.name = name
            task.description = description
            task.deadline = deadline
            task.priority = priority
        else:
            print("Invalid task index.")

    def delete_task(self, index):
        if 0 <= index < len(self.tasks):
            del self.tasks[index]
        else:
            print("Invalid task index.")

    def mark_task_completed(self, index):
        if 0 <= index < len(self.tasks):
            self.tasks[index].mark_completed()
        else:
            print("Invalid task index.")

    def filter_tasks(self, priority=None, completed=None):
        filtered_tasks = self.tasks
        if priority:
            filtered_tasks = [task for task in filtered_tasks if task.priority == priority]
        if completed is not None:
            filtered_tasks = [task for task in filtered_tasks if task.completed == completed]
        return filtered_tasks

    def export_tasks(self, filename):
        with open(filename, 'w') as f:
            json.dump([task.to_dict() for task in self.tasks], f)

def main():
    todo_list = TodoList()
    
    while True:
        print("\nTo-Do List Application")
        print("1. Add Task")
        print("2. Update Task")
        print("3. Delete Task")
        print("4. Mark Task as Completed")
        print("5. Filter Tasks")
        print("6. Export Tasks")
        print("7. Show All Tasks")
        print("8. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            name = input("Enter task name: ")
            description = input("Enter task description: ")
            deadline = input("Enter task deadline (YYYY-MM-DD): ")
            priority = input("Enter task priority (high, medium, low): ")
            todo_list.add_task(name, description, deadline, priority)

        elif choice == '2':
            index = int(input("Enter task index to update (0-indexed): "))
            name = input("Enter new task name: ")
            description = input("Enter new task description: ")
            deadline = input("Enter new task deadline (YYYY-MM-DD): ")
            priority = input("Enter new task priority (high, medium, low): ")
            todo_list.update_task(index, name, description, deadline, priority)

        elif choice == '3':
            index = int(input("Enter task index to delete (0-indexed): "))
            todo_list.delete_task(index)

        elif choice == '4':
            index = int(input("Enter task index to mark as completed (0-indexed): "))
            todo_list.mark_task_completed(index)

        elif choice == '5':
            priority = input("Filter by priority (high, medium, low) or leave empty: ")
            completed = input("Filter by completed status (yes/no) or leave empty: ")
            completed = completed == 'yes' if completed else None 
            filtered_tasks = todo_list.filter_tasks(priority, completed)
            for idx, task in enumerate(filtered_tasks):
                status = "✔️ Completed" if task.completed else "❌ Not Completed"
                print(f"{idx}. {task.name} - {status} (Priority: {task.priority}, Deadline: {task.deadline})")

        elif choice == '6':
            filename = input("Enter filename to export tasks (e.g., tasks.json): ")
            todo_list.export_tasks(filename)
            print(f"Tasks exported to {filename}")

        elif choice == '7':
            for idx, task in enumerate(todo_list.tasks):
                status = "✔️ Completed" if task.completed else "❌ Not Completed"
                print(f"{idx}. {task.name} - {status} (Priority: {task.priority}, Deadline: {task.deadline})")

        elif choice == '8':
            break

if __name__ == "__main__":
    main()# -
