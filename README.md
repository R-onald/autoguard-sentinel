# autoguard-sentinel
Smart vehicle safety and fuel estimation system with real-time monitoring and analytics .


# AutoGuard Sentinel & Smart Fuel Estimator

## 🚗 Overview
A dual-purpose smart car accessory that:
- Monitors vehicle suspension and braking
- Provides real-time alerts to mobile devices
- Estimates fuel consumption and trip costs

## 🛠️ Features
- Suspension & brake monitoring with sound & video recording
- Smart fuel estimation: calculates fuel needed or distance per budget
- Fuel type detection & automatic alerts
- Monthly statistics and email reports
- Waterproof & shockproof design

## 🛠️ Tech Stack
- Backend: Java / Python
- GUI: Tkinter (desktop app) / Java Swing
- Database: SQLite / PostgreSQL
- Integration: Email notifications, IoT sensors

## 📷 Screenshots / Diagrams
(Upload images to `docs/` folder)

## 🚀 Future Improvements
- Mobile app integration
- AI-powered predictive maintenance




import tkinter as tk
from tkinter import messagebox, simpledialog
import random
import datetime

class AutoGuardApp:
    def __init__(self, root):
        self.root = root
        self.root.title("AutoGuard Sentinel + Smart Fuel Estimator")
        self.root.geometry("700x450")

        # Tabs
        self.tab_frame = tk.Frame(root)
        self.tab_frame.pack(pady=10)

        self.monitor_btn = tk.Button(self.tab_frame, text="Vehicle Monitor", command=self.vehicle_monitor)
        self.monitor_btn.grid(row=0, column=0, padx=10)

        self.fuel_btn = tk.Button(self.tab_frame, text="Fuel Estimator", command=self.fuel_estimator)
        self.fuel_btn.grid(row=0, column=1, padx=10)

        self.login_btn = tk.Button(self.tab_frame, text="Login & Fuel Check", command=self.login_and_trip)
        self.login_btn.grid(row=0, column=2, padx=10)

        # Output area
        self.output = tk.Text(root, height=20, width=85)
        self.output.pack(pady=10)

        # Simulated user credentials
        self.users = {"ronald": "password123"}

    # ---------------- Vehicle Monitoring ----------------
    def vehicle_monitor(self):
        self.output.delete("1.0", tk.END)
        suspension = random.randint(0, 100)
        braking = random.randint(0, 100)

        self.output.insert(tk.END, f"Suspension Reading: {suspension}\n")
        self.output.insert(tk.END, f"Braking Reading: {braking}\n")

        if suspension > 80:
            messagebox.showwarning("Alert!", "Suspension critical! Check your vehicle!")
        if braking > 90:
            messagebox.showwarning("Alert!", "Braking system critical!")

    # ---------------- Fuel Estimator ----------------
    def fuel_estimator(self):
        self.output.delete("1.0", tk.END)
        tank_capacity = 50
        current_fuel = 10
        fuel_price_per_liter = 22
        budget = 200

        fuel_needed = tank_capacity - current_fuel
        cost_to_fill = fuel_needed * fuel_price_per_liter
        distance_per_budget = (budget / fuel_price_per_liter) * 10

        self.output.insert(tk.END, f"Current Fuel: {current_fuel}L\n")
        self.output.insert(tk.END, f"Fuel Needed to Fill Tank: {fuel_needed}L\n")
        self.output.insert(tk.END, f"Cost to Fill Tank: R{cost_to_fill}\n")
        self.output.insert(tk.END, f"Distance you can drive for R{budget}: {distance_per_budget} km\n")

        wrong_fuel = random.choice([True, False])
        if wrong_fuel:
            messagebox.showerror("Alert!", "Wrong fuel type detected! Tank locked.")

    # ---------------- Login & Trip / Fuel Check ----------------
    def login_and_trip(self):
        self.output.delete("1.0", tk.END)

        # Ask for username & password
        username = simpledialog.askstring("Login", "Enter username:")
        password = simpledialog.askstring("Login", "Enter password:", show="*")

        if username in self.users and self.users[username] == password:
            self.output.insert(tk.END, f"Welcome {username}! ✅\n")
        else:
            messagebox.showerror("Login Failed", "Incorrect username or password!")
            return

        # Ask for trip info
        trip_distance = simpledialog.askfloat("Trip Info", "Enter trip distance (km):")
        fuel_station = simpledialog.askstring("Fuel Station", "Enter fuel station name:")
        fuel_type = simpledialog.askstring("Fuel Type", "Enter fuel type pumped (Petrol/Diesel):")

        # Simulate correct vs wrong fuel (random for now)
        correct_fuel_type = "Petrol"
        if fuel_type.lower() != correct_fuel_type.lower():
            messagebox.showerror("Alert!", f"Wrong fuel type detected at {fuel_station}! Tank locked!")
            fuel_status = "Wrong Fuel"
        else:
            messagebox.showinfo("Fuel OK", f"Fuel at {fuel_station} verified ✅")
            fuel_status = "Correct Fuel"

        # Display trip summary
        self.output.insert(tk.END, f"\nTrip Summary:\n")
        self.output.insert(tk.END, f"Date & Time: {datetime.datetime.now()}\n")
        self.output.insert(tk.END, f"Distance: {trip_distance} km\n")
        self.output.insert(tk.END, f"Fuel Station: {fuel_station}\n")
        self.output.insert(tk.END, f"Fuel Status: {fuel_status}\n")

if __name__ == "__main__":
    root = tk.Tk()
    app = AutoGuardApp(root)
    root.mainloop()


