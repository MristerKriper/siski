# Запуск файлов

## Описание

Это приложение позволяет добавлять файлы и папки в список и запускать их все одновременно одним кликом. Удобно для быстрого открытия нескольких программ или папок вместе.

## Как пользоваться

1. Добавляйте файлы кнопкой **Добавить файл**.
2. Добавляйте папки кнопкой **Добавить папку**.
3. Выбирайте элементы и удаляйте их кнопкой **Удалить выделенное**.
4. Запускайте все добавленные элементы кнопкой **Запустить все**.


## Как запустить

- Запустите скрипт с Python 3:  
  `python siska.py`

- Или используйте готовый исполняемый файл `siska.exe` 

---
[Uploading siskimport os
import sys
import subprocess
import tkinter as tk
from tkinter import ttk, filedialog, messagebox
import tkinter.font as tkfont

class App(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Запуск файлов")
        self.geometry("800x500")
        self.configure(bg="#202020")

        # Определяем базовый путь для иконки в зависимости от запуска
        if getattr(sys, 'frozen', False):
            # Запуск из exe
            base_path = sys._MEIPASS
        else:
            # Запуск из скрипта
            base_path = os.path.abspath(".")

        icon_path = os.path.join(base_path, "загруженное.ico")
        try:
            self.iconbitmap(icon_path)
        except Exception as e:
            print(f"Не удалось загрузить иконку: {e}")

        available_fonts = tkfont.families()
        if "GRUNT_v1" in available_fonts:
            self.app_font = ("GRUNT_v1", 11)
            self.btn_font = ("GRUNT_v1", 12, "bold")
        else:
            self.app_font = ("Segoe UI", 11)
            self.btn_font = ("Segoe UI", 12, "bold")

        style = ttk.Style(self)
        style.theme_use('clam')

        style.configure(
            "Treeview",
            background="#202020",
            fieldbackground="#202020",
            foreground="#337418",
            rowheight=28,
            font=self.app_font
        )
        style.map("Treeview", background=[("selected", "#1E4018")])

        style.configure(
            "TButton",
            background="#337418",
            foreground="#202020",
            font=self.btn_font,
            padding=10,
            relief="flat"
        )
        style.map(
            "TButton",
            background=[("active", "#285B14"), ("pressed", "#1E4018")],
            foreground=[("!disabled", "#202020"), ("active", "#202020"), ("pressed", "#202020")]
        )

        self.tree = ttk.Treeview(
            self, columns=("path",), show="tree", selectmode="extended"
        )
        self.tree.heading("#0", text="Путь")
        self.tree.column("#0", width=750)
        self.tree.pack(padx=10, pady=10, fill=tk.BOTH, expand=True)

        btn_frame = ttk.Frame(self)
        btn_frame.pack(fill=tk.X, padx=10, pady=(0, 10))
        btn_frame.configure(style="My.TFrame")

        style.configure("My.TFrame", background="#202020")

        self.btn_add_file = ttk.Button(btn_frame, text="Добавить файл", command=self.add_file)
        self.btn_add_file.grid(row=0, column=0, padx=5, pady=5)

        self.btn_add_folder = ttk.Button(btn_frame, text="Добавить папку", command=self.add_folder)
        self.btn_add_folder.grid(row=0, column=1, padx=5, pady=5)

        self.btn_remove = ttk.Button(btn_frame, text="Удалить выделенное", command=self.remove_selected)
        self.btn_remove.grid(row=0, column=2, padx=5, pady=5)

        self.btn_run = ttk.Button(self, text="Запустить все", command=self.run_all)
        self.btn_run.pack(pady=(10, 10), ipady=8)

        self.paths = []

    def add_file(self):
        file_path = filedialog.askopenfilename()
        if file_path:
            self.paths.append(file_path)
            self.tree.insert("", tk.END, text=file_path)

    def add_folder(self):
        folder_path = filedialog.askdirectory()
        if folder_path:
            self.paths.append(folder_path)
            self.tree.insert("", tk.END, text=folder_path)

    def remove_selected(self):
        selected_items = self.tree.selection()
        if not selected_items:
            messagebox.showinfo("Инфо", "Выберите элемент для удаления")
            return
        for item in reversed(selected_items):
            idx = self.tree.index(item)
            self.tree.delete(item)
            del self.paths[idx]

    def run_all(self):
        if not self.paths:
            messagebox.showinfo("Инфо", "Список пуст")
            return

        for path in self.paths:
            try:
                if os.path.isdir(path):
                    subprocess.Popen(['explorer', path], shell=True)

                elif path.lower().endswith('.lnk'):
                    subprocess.Popen([path], shell=True)

                else:
                    if os.path.basename(path).lower() == 'update.exe':
                        subprocess.Popen([path, '--processStart', 'Discord.exe'], shell=True)
                    else:
                        subprocess.Popen([path], shell=True)

            except Exception as e:
                messagebox.showerror("Ошибка", f"Не удалось запустить {path}:\n{e}")

if __name__ == "__main__":
    app = App()
    app.mainloop()
a.py…]()



