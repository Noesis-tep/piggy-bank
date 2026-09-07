# piggy-bank
import tkinter as tk
from tkinter import messagebox, ttk

class MoneyGoal:
    def __init__(self):
        self.target = 2100
        self.current = 0
        self.setup_window()
    
    def setup_window(self):
        self.window = tk.Tk()
        self.window.title("Копилка")
        self.window.geometry("400x300")
        
        tk.Label(self.window, text=f"Цель: {self.target} руб.", 
                 font=("Arial", 14)).pack(pady=10)
        
        self.balance_label = tk.Label(self.window, text=f"Баланс: 0 руб.",
                                       font=("Arial", 16, "bold"), fg="blue")
        self.balance_label.pack(pady=10)
        
 
        self.progress = ttk.Progressbar(self.window, length=300, mode='determinate')
        self.progress.pack(pady=10)
        
        tk.Label(self.window, text="Сколько добавить?").pack()
        self.entry = tk.Entry(self.window, width=20)
        self.entry.pack(pady=5)
        
        tk.Button(self.window, text="+ Добавить", command=self.add_money,
                 bg="green", fg="white", padx=20).pack(pady=5)
        
        tk.Button(self.window, text= "src = Достичь цели авто", command=self.auto_fill,
                 bg="orange", padx=20).pack(pady=5)
        
        self.update_display()
        self.window.mainloop()
    
    def add_money(self):
        try:
            amount = int(self.entry.get())
            if amount <= 0:
                messagebox.showerror("Ошибка", "Введите положительное число")
                return
            
            self.current += amount
            self.entry.delete(0, tk.END)
            self.update_display()
            
            if self.current >= self.target:
                messagebox.showinfo("Поздравляем!", "Цель достигнута!")
                
        except ValueError:
            messagebox.showerror("Ошибка", "Введите целое число!")
    
    def auto_fill(self):
        while self.current < self.target:
            self.current = min(self.current + 100, self.target)
            self.update_display()
            self.window.update()  
            self.window.after(100)  
        
        messagebox.showinfo("Готово!", f"Цель достигнута! Баланс: {self.current} руб.")
    
    def update_display(self):
        self.balance_label.config(text=f"Баланс: {self.current} руб.")
        percent = (self.current / self.target) * 100
        self.progress['value'] = percent
        
        if self.current >= self.target:
            self.balance_label.config(fg="green")

if __name__ == "__main__":
    app = MoneyGoal()
