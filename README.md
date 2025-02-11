# Quiz-app
Quiz Game

import tkinter as tk
from tkinter import messagebox
import random

class QuizApp:
    def __init__(self, root):
        self.root = root
        self.root.title("🧠 Quizzer Mania ⏳")
        self.root.geometry("500x450")
        self.root.configure(bg="#1e1e2e")
        
        self.header_label = tk.Label(self.root, text="Quizzer Mania", font=("Arial", 16, "bold"), bg="#1e1e2e", fg="cyan")
        self.header_label.pack(pady=5)
        
        self.rishi_label = tk.Label(self.root, text="Rishi's Gamer", font=("Arial", 10, "italic"), bg="#1e1e2e", fg="yellow", anchor="e")
        self.rishi_label.pack(fill="x")
        
        self.questions = [
            {"question": "What is the capital of France?", "options": ["Berlin", "Madrid", "Paris", "Rome"], "answer": "Paris"},
            {"question": "What is 5 + 7?", "options": ["10", "12", "15", "9"], "answer": "12"},
            {"question": "Who wrote 'Hamlet'?", "options": ["Shakespeare", "Hemingway", "Tolkien", "Austen"], "answer": "Shakespeare"},
            {"question": "What is the largest planet in our solar system?", "options": ["Earth", "Mars", "Jupiter", "Saturn"], "answer": "Jupiter"},
            {"question": "Which element has the chemical symbol 'O'?", "options": ["Oxygen", "Gold", "Osmium", "Silver"], "answer": "Oxygen"},
            {"question": "What is the speed of light?", "options": ["300,000 km/s", "150,000 km/s", "1,000 km/s", "500,000 km/s"], "answer": "300,000 km/s"},
            {"question": "Who painted the Mona Lisa?", "options": ["Van Gogh", "Picasso", "Da Vinci", "Rembrandt"], "answer": "Da Vinci"},
            {"question": "What is the smallest prime number?", "options": ["1", "2", "3", "5"], "answer": "2"},
            {"question": "Which gas do plants use for photosynthesis?", "options": ["Oxygen", "Nitrogen", "Carbon Dioxide", "Hydrogen"], "answer": "Carbon Dioxide"},
            {"question": "What is the hardest natural substance on Earth?", "options": ["Gold", "Iron", "Diamond", "Quartz"], "answer": "Diamond"}
        ]
        
        self.max_questions = len(self.questions)
        self.create_widgets()
        self.reset_quiz()
        
    def create_widgets(self):
        self.start_button = tk.Button(self.root, text="Start Quiz", font=("Arial", 14, "bold"), command=self.start_quiz, bg="#4CAF50", fg="white")
        self.start_button.pack(pady=20)
        
        self.question_label = tk.Label(self.root, text="", font=("Arial", 14, "bold"), bg="#1e1e2e", fg="white", wraplength=400)
        self.options = []
        for i in range(4):
            btn = tk.Button(self.root, text="", font=("Arial", 12), width=20, command=lambda i=i: self.check_answer(i), bg="#4CAF50", fg="white")
            self.options.append(btn)
        
        self.timer_label = tk.Label(self.root, text="", font=("Arial", 12, "bold"), bg="#1e1e2e", fg="yellow")
        self.score_label = tk.Label(self.root, text="", font=("Arial", 12, "bold"), bg="#1e1e2e", fg="white")
        self.question_counter_label = tk.Label(self.root, text="", font=("Arial", 12, "bold"), bg="#1e1e2e", fg="white")
        
        self.restart_button = tk.Button(self.root, text="Restart Quiz", font=("Arial", 12, "bold"), command=self.reset_quiz, bg="#FF9800", fg="white")
    
    def reset_quiz(self):
        random.shuffle(self.questions)
        self.current_question = 0
        self.score = 0
        self.time_left = 30
        self.start_button.pack(pady=20)
        self.question_label.pack_forget()
        self.timer_label.pack_forget()
        self.score_label.pack_forget()
        self.question_counter_label.pack_forget()
        self.restart_button.pack_forget()
        for btn in self.options:
            btn.pack_forget()
        
    def start_quiz(self):
        self.start_button.pack_forget()
        self.question_label.pack(pady=10)
        for btn in self.options:
            btn.pack(pady=5)
        self.timer_label.pack(pady=10)
        self.score_label.pack(pady=5)
        self.question_counter_label.pack(pady=5)
        self.load_question()
        self.update_timer()
        
    def load_question(self):
        if self.current_question < self.max_questions:
            q = self.questions[self.current_question]
            self.question_label.config(text=q["question"])
            for i in range(4):
                self.options[i].config(text=q["options"][i])
            self.time_left = 30
            self.score_label.config(text=f"Score: {self.score}/{self.max_questions}")
            self.question_counter_label.config(text=f"Question {self.current_question + 1}/{self.max_questions}")
        else:
            self.show_result()
    
    def check_answer(self, index):
        if self.questions[self.current_question]["options"][index] == self.questions[self.current_question]["answer"]:
            self.score += 1
        self.current_question += 1
        self.load_question()
    
    def update_timer(self):
        if self.time_left > 0:
            self.time_left -= 1
            self.timer_label.config(text=f"Time left: {self.time_left}s")
            self.root.after(1000, self.update_timer)
        else:
            self.current_question += 1
            self.load_question()
    
    def show_result(self):
        if self.score >= 5:
            messagebox.showinfo("🎉 You Won!", f"🎇 Congratulations! You scored {self.score}/{self.max_questions} 🎆")
        else:
            messagebox.showinfo("Quiz Over!", f"Your Score: {self.score}/{self.max_questions}")
        
        self.question_label.pack_forget()
        self.timer_label.pack_forget()
        self.score_label.pack_forget()
        self.question_counter_label.pack_forget()
        for btn in self.options:
            btn.pack_forget()
        self.restart_button.pack(pady=20)

if __name__ == "__main__":
    root = tk.Tk()
    app = QuizApp(root)
    root.mainloop()
