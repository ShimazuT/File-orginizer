# File-orginizer
# File Organizer with Undo and GUI

This app organizes files in a selected folder by extension, moves them to subfolders, and allows undoing the operation.

## Features
- Organize files by type
- Undo file moves
- GUI using Tkinter
- Error handling

## here is a code
```bash

import os
import shutil
import logging
from datetime import datetime
import tkinter as tk
from tkinter import filedialog, messagebox

# Configure logging
logging.basicConfig(filename='file_mover.log', level=logging.INFO, format='%(asctime)s - %(message)s')

class FileOrganizer:
    def __init__(self):
        self.history = []
        self.root = tk.Tk()
        self.root.title("File Organizer")
        self.source_folder = ""
        
        self.create_widgets()

    def create_widgets(self):
        self.select_button = tk.Button(self.root, text="Select Folder", command=self.select_folder)
        self.select_button.pack(pady=10)

        self.start_button = tk.Button(self.root, text="Start Organizing", command=self.organize_files)
        self.start_button.pack(pady=10)

        self.stop_button = tk.Button(self.root, text="Stop", command=self.stop_organizing)
        self.stop_button.pack(pady=10)

    def select_folder(self):
        self.source_folder = filedialog.askdirectory()
        if not self.source_folder:
            messagebox.showwarning("Warning", "No folder selected!")

    def organize_files(self):
        if not self.source_folder:
            messagebox.showwarning("Warning", "Please select a folder first!")
            return
        
        for filename in os.listdir(self.source_folder):
            file_path = os.path.join(self.source_folder, filename)
            if os.path.isfile(file_path):
                ext = filename.split('.')[-1]
                dest_folder = os.path.join(self.source_folder, ext)
                os.makedirs(dest_folder, exist_ok=True)
                dest_path = os.path.join(dest_folder, filename)

                try:
                    shutil.move(file_path, dest_path)
                    logging.info(f'Moved: {file_path} to {dest_path}')
                    self.history.append((file_path, dest_path))
                except Exception as e:
                    logging.error(f'Error moving {file_path}: {e}')
                    messagebox.showerror("Error", f"Error moving {file_path}: {e}")

    def stop_organizing(self):
        self.history.clear()
        messagebox.showinfo("Info", "Organizing stopped and history cleared.")

    def run(self):
        self.root.mainloop()

if __name__ == "__main__":
    organizer = FileOrganizer()
    organizer.run()
