### Advanced Calculator
## Advanced Calculator
# Advanced Calculator
```bash
import tkinter as tk
import math

def click(event):
    global expression
    text = event.widget.cget("text")
    if text == "=":
        try:
            result = eval(expression)
            input_var.set(result)
            history_list.insert(tk.END, f"{expression} = {result}")
            expression = str(result)
        except Exception as e:
            input_var.set("Error")
            expression = ""
    elif text == "C":
        expression = ""
        input_var.set("")
    elif text == "Clear History":
        history_list.delete(0, tk.END)
    elif text in ["sqrt", "sin", "cos", "tan", "log", "^"]:
        try:
            if text == "sqrt":
                result = math.sqrt(float(expression))
            elif text == "sin":
                result = math.sin(math.radians(float(expression)))
            elif text == "cos":
                result = math.cos(math.radians(float(expression)))
            elif text == "tan":
                result = math.tan(math.radians(float(expression)))
            elif text == "log":
                result = math.log(float(expression))
            elif text == "^":
                expression += "**"  # Use Python's exponentiation operator
                input_var.set(expression)
                return
            input_var.set(result)
            history_list.insert(tk.END, f"{text}({expression}) = {result}")
            expression = str(result)
        except Exception as e:
            input_var.set("Error")
            expression = ""
    else:
        expression += text
        input_var.set(expression)

def key_press(event):
    if event.char in "0123456789+-*/().":
        expression += event.char
        input_var.set(expression)
    elif event.char == "=":
        click(event)
    elif event.char == "\r":  # Enter key
        click(event)
    elif event.char == "c" or event.char == "C":
        expression = ""
        input_var.set("")
    elif event.char == "h" or event.char == "H":
        history_list.delete(0, tk.END)
```

expression = ""
root = tk.Tk()
root.title("Advanced Calculator")
root.geometry("400x600")
root.configure(bg="#2E2E2E")

input_var = tk.StringVar()
input_field = tk.Entry(root, textvar=input_var, font=("Arial", 24), justify=tk.RIGHT, bd=10, insertwidth=2, width=14, borderwidth=4)
input_field.pack(fill=tk.BOTH, ipadx=8, pady=20, padx=20)

button_frame = tk.Frame(root, bg="#2E2E2E")
button_frame.pack()

buttons = [
    ["7", "8", "9", "/"],
    ["4", "5", "6", "*"],
    ["1", "2", "3", "-"],
    ["C", "0", "=", "+"],
    ["sqrt", "sin", "cos", "tan"],
    ["log", "^", "Clear History"]
]

for row in buttons:
    frame = tk.Frame(button_frame, bg="#2E2E2E")
    frame.pack(side=tk.TOP)
    for btn in row:
        button = tk.Button(frame, text=btn, font=("Arial", 18), width=5, height=2, bg="#4CAF50", fg="white", activebackground="#45a049")
        button.pack(side=tk.LEFT, padx=5, pady=5)
        button.bind("<Button-1>", click)

# History Listbox
history_frame = tk.Frame(root, bg="#2E2E2E")
history_frame.pack(fill=tk.BOTH, expand=True)

history_label = tk.Label(history_frame, text="History", bg="#2E2E2E", fg="white", font=("Arial", 16))
history_label.pack()

history_list = tk.Listbox(history_frame, bg="#4CAF50", fg="white", font=("Arial", 14))
history_list.pack(fill=tk.BOTH, expand=True)

# Bind keyboard events
root.bind("<Key>", key_press)

root.mainloop()
