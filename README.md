# 🧮 Scientific Calculator (Kivy)

A *beautiful scientific calculator* built using *Python (Kivy)*.  
Designed for mobile with *large buttons & clean UI*.

## 🚀 Features
- Basic operations: +, -, ×, ÷
- Scientific functions: sin, cos, tan, log, exp, sqrt, pi
- Large responsive buttons 📱
- Dark theme UI
- Creator info displayed at the top

## 📸 Screenshot
![Calculator Screenshot](screenshot.png)

## ⚙ Installation
```bash
# Clone the repo
#git clone #https://github.com/Anishkushwaha02/Sc#ientific-Calculator v1.1.git
#cd Scientific-Calculator

# Install requirements
# pip install kivy


import os
import math
from kivy.config import Config
from kivy.app import App
from kivy.uix.gridlayout import GridLayout
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput
from kivy.uix.label import Label
from kivy.uix.boxlayout import BoxLayout

# 🔹 Disable unnecessary providers & logs
os.environ["KIVY_NO_ARGS"] = "1"
os.environ["KIVY_AUDIO"] = "sdl2"
os.environ["KIVY_IMAGE"] = "sdl2,pil"
os.environ["KIVY_TEXT"] = "sdl2"
os.environ["KIVY_VIDEO"] = ""
os.environ["KIVY_LOG_LEVEL"] = "warning"

Config.set("kivy", "keyboard_mode", "system")


# -------------------------------
# 🚀 Scientific Calculator
# -------------------------------
class Calculator(GridLayout):
    def __init__(self, **kwargs):  # ✅ fixed __init__
        super().__init__(**kwargs)
        self.cols = 1

        # 🔹 Creator Details
        self.add_widget(Label(
            text="Scientific Calculator\n*\n[] Creator : Anish Kushwaha\n[*] Email : Anish_Kushwaha@proton.me",
            font_size=20,
            halign="center",
            valign="middle",
            size_hint_y=None,
            height=100,
            color=(1, 1, 0, 1)
        ))

        # 🔹 Display
        self.display = TextInput(
            multiline=False,
            font_size=100,
            readonly=False,
            halign="right",
            size_hint_y=None,
            height=250
        )
        self.add_widget(self.display)

        # 🔹 Buttons layout
        self.buttons = GridLayout(cols=4, spacing=5, size_hint_y=0.9)
        buttons = [
            "7", "8", "9", "/",
            "4", "5", "6", "*",
            "1", "2", "3", "-",
            "0", ".", "=", "+",
            "sin", "cos", "tan", "sqrt",
            "log", "exp", "pi", "C",
            "^"
        ]

        for label in buttons:
            btn = Button(
                text=label,
                font_size=65,
                background_color=(0.1, 0.3, 0.5, 1),
                color=(1, 1, 1, 1)
            )
            btn.bind(on_press=self.on_button_press)
            self.buttons.add_widget(btn)

        self.add_widget(self.buttons)

    # 🔹 Button Events
    def on_button_press(self, instance):
        text = instance.text

        if text == "C":
            self.display.text = ""
        elif text == "=":
            try:
                # ✅ Replace ^ with ** (Python exponent operator)
                expr = self.display.text.replace("^", "**")

                # ✅ Safe allowed functions/constants
                allowed_funcs = {k: getattr(math, k) for k in dir(math) if not k.startswith("_")}
                allowed_funcs["pi"] = math.pi
                allowed_funcs["e"] = math.e

                self.display.text = str(eval(expr, {"__builtins__": None}, allowed_funcs))
            except Exception:
                self.display.text = "Error"
        else:
            self.display.text += text


# -------------------------------
# 🚀 App Class
# -------------------------------
class CalculatorApp(App):
    def build(self):
        layout = BoxLayout(orientation="vertical", padding=10, spacing=10)
        layout.add_widget(Calculator())
        return layout


# -------------------------------
# Run App
# -------------------------------
if __name__ == "__main__":  # ✅ fixed
    CalculatorApp().run()



