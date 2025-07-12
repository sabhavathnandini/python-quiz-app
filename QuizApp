import tkinter as tk
from tkinter import messagebox, simpledialog
import random

# Quiz Questions stored as a Python dictionary
questions = {
    "Who created Python?": {
        "options": ["Guido van Rossum", "Elon Musk", "Bill Gates", "Mark Zuckerberg"],
        "answer": "A"
    },
    "What year was Python created?": {
        "options": ["1989", "1991", "2000", "2016"],
        "answer": "B"
    },
    "Python is tributed to which comedy group?": {
        "options": ["Lonely Island", "Smosh", "Monty Python", "SNL"],
        "answer": "C"
    },
    "Is the Earth round?": {
        "options": ["True", "False", "Sometimes", "What's Earth?"],
        "answer": "A"
    }
}

# Global leaderboard dictionary
leaderboard = {}

class QuizApp:
    def __init__(self, root, username):
        self.root = root
        self.root.title("Python Dictionary Quiz")
        self.username = username

        self.questions_list = list(questions.items())
        random.shuffle(self.questions_list)
        self.q_index = 0
        self.score = 0

        self.time_left = 15
        self.timer_id = None

        self.question_label = tk.Label(root, text="", font=("Arial", 16), wraplength=500, justify="left")
        self.question_label.pack(pady=20)

        self.timer_label = tk.Label(root, text="Time left: 15s", font=("Arial", 14), fg="red")
        self.timer_label.pack()

        self.radio_buttons = []
        self.selected_option = tk.StringVar()

        for i in range(4):
            rb = tk.Radiobutton(root, text="", variable=self.selected_option, value=chr(65+i),
                                font=("Arial", 14), anchor="w", padx=20)
            rb.pack(fill="x", padx=40, pady=5)
            self.radio_buttons.append(rb)

        self.submit_button = tk.Button(root, text="Submit Answer", command=self.submit_answer, font=("Arial", 14))
        self.submit_button.pack(pady=20)

        self.display_question()

    def display_question(self):
        if self.timer_id:
            self.root.after_cancel(self.timer_id)

        self.time_left = 15
        self.update_timer()

        q_text, q_data = self.questions_list[self.q_index]
        self.question_label.config(text=f"Q{self.q_index + 1}: {q_text}")
        self.selected_option.set(None)
        for i, option in enumerate(q_data["options"]):
            self.radio_buttons[i].config(text=f"{chr(65 + i)}. {option}")

    def update_timer(self):
        self.timer_label.config(text=f"Time left: {self.time_left}s")
        if self.time_left > 0:
            self.time_left -= 1
            self.timer_id = self.root.after(1000, self.update_timer)
        else:
            messagebox.showinfo("Time's Up!", "You ran out of time for this question.")
            self.submit_answer(auto_skip=True)

    def submit_answer(self, auto_skip=False):
        if self.timer_id:
            self.root.after_cancel(self.timer_id)

        guess = self.selected_option.get()
        if not guess and not auto_skip:
            messagebox.showwarning("No Selection", "Please select an option before submitting.")
            self.update_timer()  # resume timer
            return

        correct = self.questions_list[self.q_index][1]["answer"]
        if guess == correct:
            self.score += 1

        self.q_index += 1
        if self.q_index < len(self.questions_list):
            self.display_question()
        else:
            self.show_result()

    def show_result(self):
        percent = int((self.score / len(self.questions_list)) * 100)
        leaderboard[self.username] = self.score
        messagebox.showinfo("Quiz Over", f"{self.username}, your score is {self.score}/{len(self.questions_list)} ({percent}%)")
        self.show_leaderboard()
        self.root.withdraw()  # Just hide the quiz window instead of destroying it

    def show_leaderboard(self):
        leaderboard_window = tk.Toplevel(self.root)
        leaderboard_window.title("Leaderboard")
        leaderboard_window.geometry("300x300")

        title = tk.Label(leaderboard_window, text=" Leaderboard ", font=("Arial", 16, "bold"))
        title.pack(pady=10)

        sorted_leaderboard = sorted(leaderboard.items(), key=lambda x: x[1], reverse=True)

        for rank, (user, score) in enumerate(sorted_leaderboard, start=1):
            label = tk.Label(leaderboard_window, text=f"{rank}. {user} - {score} pts", font=("Arial", 12))
            label.pack()

        def close_app():
            leaderboard_window.destroy()
            self.root.destroy()

        tk.Button(leaderboard_window, text="Close", command=close_app).pack(pady=10)


def start_quiz():
    root = tk.Tk()
    username = simpledialog.askstring("Login", "Enter your username:", parent=root)
    if not username:
        root.destroy()
        return
    app = QuizApp(root, username)
    root.mainloop()


# Start the app
if __name__ == "__main__":
    start_quiz()
