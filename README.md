import tkinter as tk

class Task:
    def __init__(self, description, due_date, priority):
        self.description = description
        self.due_date = due_date
        self.priority = priority
        self.completed = False

class ToDoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("To Do List")

        title_label = tk.Label(root, text="To Do List", font=("Arial", 16, "bold"), fg="black")
        title_label.pack(pady=10)

        self.tasks = []
        self.completed_tasks = []

        self.task_entry = tk.Entry(root, width=40, bg="ivory")
        self.task_entry.pack(pady=10)

        self.due_date_entry = tk.Entry(root, width=10)
        self.due_date_entry.pack()

        self.priority_entry = tk.Entry(root, width=10)
        self.priority_entry.pack()

        self.add_button = tk.Button(root, text="Add Task", command=self.add_task, bg="orange")
        self.add_button.pack()

        self.task_listbox = tk.Listbox(root, width=60, bg="ivory")
        self.task_listbox.pack()

        self.completed_listbox = tk.Listbox(root, width=60, bg="lightgreen")  # Listbox for completed tasks
        self.completed_listbox.pack()

        self.complete_button = tk.Button(root, text="Mark as Completed", command=self.mark_as_completed, bg="pink")
        self.complete_button.pack()

        self.update_button = tk.Button(root, text="Update Task", command=self.update_task, bg="yellow")
        self.update_button.pack()

        self.remove_button = tk.Button(root, text="Remove Task", command=self.remove_task, bg="lightblue")
        self.remove_button.pack()

        self.update_entry = tk.Entry(root, width=40, bg="ivory")
        self.update_entry.pack()

        self.update_due_date_entry = tk.Entry(root, width=10)
        self.update_due_date_entry.pack()

        self.update_priority_entry = tk.Entry(root, width=10)
        self.update_priority_entry.pack()

    def add_task(self):
        task_description = self.task_entry.get()
        due_date = self.due_date_entry.get()
        priority = self.priority_entry.get()

        if task_description:
            task = Task(task_description, due_date, priority)
            self.tasks.append(task)
            self.task_listbox.insert(tk.END, self.task_display(task))
            self.clear_task_entry_fields()

    def mark_as_completed(self):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            task_index = selected_index[0]
            task = self.tasks[task_index]
            task.completed = True
            self.completed_tasks.append(task)
            self.tasks.pop(task_index)
            self.update_task_listbox()
            self.update_completed_listbox()

    def update_task(self):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            task_index = selected_index[0]
            new_description = self.update_entry.get()
            if new_description:
                task = self.tasks[task_index]
                task.due_date = self.update_due_date_entry.get()
                task.priority = self.update_priority_entry.get()
                task.description = new_description
                self.update_task_listbox()
                self.update_entry.delete(0, tk.END)
                self.update_due_date_entry.delete(0, tk.END)
                self.update_priority_entry.delete(0, tk.END)

    def remove_task(self):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            task_index = selected_index[0]
            self.tasks.pop(task_index)
            self.update_task_listbox()

    def update_task_listbox(self):
        self.task_listbox.delete(0, tk.END)
        for task in self.tasks:
            self.task_listbox.insert(tk.END, self.task_display(task))

    def update_completed_listbox(self):
        self.completed_listbox.delete(0, tk.END)
        for task in self.completed_tasks:
            self.completed_listbox.insert(tk.END, self.task_display(task))

    def task_display(self, task):
        return f"{task.description} | Due Date: {task.due_date} | Priority: {task.priority}"

    def clear_task_entry_fields(self):
        self.task_entry.delete(0, tk.END)
        self.due_date_entry.delete(0, tk.END)
        self.priority_entry.delete(0, tk.END)

if __name__ == "__main__":
    root = tk.Tk()
    app = ToDoApp(root)
    root.mainloop()
