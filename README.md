import json
import os

FILE_NAME = 'todo_data.json'

def load_data():
    if not os.path.exists(FILE_NAME):
        return []
    with open(FILE_NAME, 'r') as f:
        return json.load(f)

def save_data(tasks):
    with open(FILE_NAME, 'w') as f:
        json.dump(tasks, f, indent=2)

def list_tasks(tasks):
    if not tasks:
        print("lagi nyantai nih bro.")
        return
    for i, task in enumerate(tasks, start=1):
        status = "clear" if task['done'] else " "
        print(f"{i}. [{status}] {task['task']}")

def add_task(tasks, description):
    tasks.append({'task': description, 'done': False})
    save_data(tasks)
    print(f"Tugas '{description}' ditambahkan.")

def mark_done(tasks, index):
    if 0 <= index < len(tasks):
        tasks[index]['done'] = True
        save_data(tasks)
        print(f"Tugas '{tasks[index]['task']}' ditandai selesai.")
    else:
        print("Indeks tidak valid.")

def delete_task(tasks, index):
    if 0 <= index < len(tasks):
        removed = tasks.pop(index)
        save_data(tasks)
        print(f"Tugas '{removed['task']}' dihapus.")
    else:
        print("Indeks tidak valid.")

def main():
    tasks = load_data()
    while True:
        print("\nMenu: list | add <tugas> | done <nomor> | delete <nomor> | Exit")
        command = input("Masukkan perintah: ").strip()

        if command == "list":
            list_tasks(tasks)
        elif command.startswith("add "):
            add_task(tasks, command[4:])
        elif command.startswith("done "):
            try:
                idx = int(command[5:]) - 1
                mark_done(tasks, idx)
            except ValueError:
                print("Format salah. Gunakan: done <nomor>")
        elif command.startswith("delete "):
            try:
                idx = int(command[7:]) - 1
                delete_task(tasks, idx)
            except ValueError:
                print("Format salah. Gunakan: delete <nomor>")
        elif command == "exit":
            print("Keluar dari aplikasi.")
            break
        else:
            print("Perintah tidak dikenali.")

if __name__ == "__main__":
    main()