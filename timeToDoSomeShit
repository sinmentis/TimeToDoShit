# !/usr/bin/env python3

import ctypes
import time as tm
import threading
from tkinter import *

RESLUTION = [600, 150]


class TimerGUI:
    def __init__(self, window):
        self.window = window
        self.window.protocol("WM_DELETE_WINDOW", self.exit_handler)

        # Set title
        self.window.title("Timer")

        # centralize the windows
        width_limit = (self.window.winfo_screenwidth() - RESLUTION[0]) / 2
        height_limit = (self.window.winfo_screenheight() - RESLUTION[1]) / 2
        self.window.geometry("{2}x{3}+{0:.0f}+{1:.0f}".format(width_limit, height_limit, RESLUTION[0], RESLUTION[1]))

        # Disable resize
        self.window.resizable(False, False)

        # Timer Thread
        self.timer_thread = None
        self.timer_running = False

        # Add Clock
        self.target_time_hms = ""  # HH:MM:SS
        self.target_time_second = 0  # second
        self.current_time_second = 0  # second
        self.cancel_flag = False
        self.clock_frame = LabelFrame(self.window, text="Clock")
        self.clock_frame.pack(anchor=N, expand="YES", fill=X, pady=10, padx=10)
        self.time_label_list = []
        for label_index in range(8):
            if label_index == 2 or label_index == 5:
                temp = Label(self.clock_frame, text=":", font=("Helvetica", 40))
                temp.pack(side=LEFT, expand=YES)
            else:
                self.time_label_list.append(Label(self.clock_frame, text="0", font=("Helvetica", 40)))
                self.time_label_list[-1].pack(side=LEFT, expand=YES)

        # Add indicater
        # self.status_frame = LabelFrame(self.window, text="Status")
        # self.status_frame.pack(anchor=NE, padx=10)
        # status_label_temp = Label(self.status_frame, text="Status:", font=("Helvetica", 15))
        # status_label_temp.pack(side=LEFT, padx=10)
        # self.status_label = Label(self.status_frame, text="STOP", font=("Helvetica", 15))
        # self.status_label.pack(side=LEFT, padx=10)

        # Listen to keyboard input
        self.input_entry = Entry(text="000000")
        self.input_entry.pack(side=TOP, expand=YES)
        self.input_entry.bind('<Key-Return>', self.keyboard_input_callback)  # "Enter" trigger this line

        # Button for start the timer
        self.start_button = Button(self.window, text="START", command=self.start_timer_thread)
        self.start_button.pack(side=TOP)

    def keyboard_input_callback(self, event):

        if self.timer_running:
            self.cancel_timer()

        # Fill if not full, reformat into HH:MM:SS
        match_result = re.findall(r'\d', self.input_entry.get())
        if len(match_result) < 6:
            match_result = ["0"] * (6 - len(match_result)) + match_result

        self.target_time_hms = ""
        for label_index in range(len(self.time_label_list)):
            self.target_time_hms += match_result[label_index]

        # update timer display and convert to seconds
        self.updata_timer_display(self.target_time_hms)
        self.target_time_second = hms_to_seconds(self.target_time_hms)

    def updata_timer_display(self, hms_string):
        for index, label in enumerate(self.time_label_list):
            label['text'] = hms_string[index]

    def start_timer_thread(self):
        if self.target_time_second != 0:
            if self.timer_running:
                self.cancel_timer()
            self.timer_running = True
            self.timer_thread = threading.Thread(target=self.start_timer)
            self.timer_thread.start()

    def start_timer(self):
        self.current_time_second = self.target_time_second
        self.updata_timer_display(seconds_to_hms(self.current_time_second))

        while self.timer_running:
            if self.current_time_second != 0:
                self.current_time_second = float("{:.2f}".format(self.current_time_second - 0.05))  # float calc thing
                self.updata_timer_display(seconds_to_hms(self.current_time_second))
                tm.sleep(0.05)
            else:
                self.timer_running = False
                if not self.cancel_flag:
                    time_up()
                self.cancel_flag = False
                sys.exit()

    def cancel_timer(self):
        self.current_time_second = 0
        self.cancel_flag = True
        tm.sleep(0.5)

    def exit_handler(self):
        self.cancel_timer()
        self.window.destroy()


def seconds_to_hms(seconds):
    return (tm.strftime('%H:%M:%S', tm.gmtime(seconds))).replace(":", "")


def hms_to_seconds(time):
    return int(time[:2]) * 3600 + int(time[2:4]) * 60 + int(time[4:])


def time_up():
    text = 'Times up'
    ctypes.windll.user32.MessageBoxW(0, text, 'Times up', 0x1000)


def main():
    window = Tk()
    TimerGUI(window)
    window.mainloop()


if __name__ == "__main__":
    main()
