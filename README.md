import tkinter as tk
import math
import random

# SOLAR SYSTEM EXPLORER
# Beginner-friendly project using only Python's built-in Tkinter.

BG = "#070b1b"
PANEL = "#101a33"
PANEL2 = "#172342"
WHITE = "#f4f7ff"
MUTED = "#aab8d6"
ACCENT = "#7c9cff"
GOLD = "#ffd166"

planets = [
    {"name":"Mercury","color":"#b8b2a8","radius":7,"type":"Terrestrial planet","order":"1st from the Sun","distance":"57.9 million km","day":"58.6 Earth days","year":"88 Earth days","moons":"0","atmosphere":"Almost no atmosphere","fact":"Mercury is the smallest planet in our Solar System."},
    {"name":"Venus","color":"#f2c078","radius":11,"type":"Terrestrial planet","order":"2nd from the Sun","distance":"108.2 million km","day":"243 Earth days","year":"225 Earth days","moons":"0","atmosphere":"Thick carbon dioxide atmosphere","fact":"Venus is the hottest planet because of its strong greenhouse effect."},
    {"name":"Earth","color":"#4b9cff","radius":12,"type":"Terrestrial planet","order":"3rd from the Sun","distance":"149.6 million km","day":"About 24 hours","year":"365.25 days","moons":"1","atmosphere":"Mostly nitrogen and oxygen","fact":"Earth is the only planet currently known to support life."},
    {"name":"Mars","color":"#f07859","radius":9,"type":"Terrestrial planet","order":"4th from the Sun","distance":"227.9 million km","day":"24.6 hours","year":"687 Earth days","moons":"2","atmosphere":"Thin carbon dioxide atmosphere","fact":"Mars is called the Red Planet because of iron-rich dust on its surface."},
    {"name":"Jupiter","color":"#e8b58a","radius":22,"type":"Gas giant","order":"5th from the Sun","distance":"778.6 million km","day":"About 9.9 hours","year":"About 11.9 Earth years","moons":"Many known moons","atmosphere":"Mostly hydrogen and helium","fact":"Jupiter is the largest planet and has a giant storm called the Great Red Spot."},
    {"name":"Saturn","color":"#e9d39a","radius":19,"type":"Gas giant","order":"6th from the Sun","distance":"1.43 billion km","day":"About 10.7 hours","year":"About 29.4 Earth years","moons":"Many known moons","atmosphere":"Mostly hydrogen and helium","fact":"Saturn's beautiful rings are made mostly of pieces of ice and rock."},
    {"name":"Uranus","color":"#8de0e8","radius":15,"type":"Ice giant","order":"7th from the Sun","distance":"2.87 billion km","day":"About 17.2 hours","year":"About 84 Earth years","moons":"Many known moons","atmosphere":"Hydrogen, helium and methane","fact":"Uranus rotates on its side compared with most other planets."},
    {"name":"Neptune","color":"#5576ff","radius":15,"type":"Ice giant","order":"8th from the Sun","distance":"4.50 billion km","day":"About 16.1 hours","year":"About 165 Earth years","moons":"Many known moons","atmosphere":"Hydrogen, helium and methane","fact":"Neptune is extremely far from the Sun and has very strong winds."}
]

root = tk.Tk()
root.title("Solar System Explorer")
root.geometry("1120x740")
root.minsize(900, 650)
root.configure(bg=BG)

header = tk.Frame(root, bg=BG)
header.pack(fill="x", padx=26, pady=(20, 12))
tk.Label(header, text="✦  SOLAR SYSTEM EXPLORER", font=("Arial", 23, "bold"), bg=BG, fg=WHITE).pack(anchor="w")
tk.Label(header, text="A little journey through our cosmic neighbourhood", font=("Arial", 11), bg=BG, fg=MUTED).pack(anchor="w", pady=(5, 0))

main = tk.Frame(root, bg=BG)
main.pack(fill="both", expand=True, padx=26, pady=(0, 22))

space_panel = tk.Frame(main, bg=PANEL, highlightthickness=1, highlightbackground="#25365c")
space_panel.pack(side="left", fill="both", expand=True, padx=(0, 16))
tk.Label(space_panel, text="THE PLANETS", font=("Arial", 11, "bold"), bg=PANEL, fg=ACCENT).pack(anchor="w", padx=16, pady=(15, 2))
tk.Label(space_panel, text="Click a planet to explore it", font=("Arial", 9), bg=PANEL, fg=MUTED).pack(anchor="w", padx=16, pady=(0, 10))
canvas = tk.Canvas(space_panel, bg="#080e22", highlightthickness=0)
canvas.pack(fill="both", expand=True, padx=12, pady=(0, 12))

info_panel = tk.Frame(main, bg=PANEL, width=330, highlightthickness=1, highlightbackground="#25365c")
info_panel.pack(side="right", fill="y")
info_panel.pack_propagate(False)

planet_title = tk.Label(info_panel, text="Earth", font=("Arial", 23, "bold"), bg=PANEL, fg=WHITE)
planet_title.pack(anchor="w", padx=20, pady=(20, 2))
planet_subtitle = tk.Label(info_panel, text="Our home planet", font=("Arial", 10), bg=PANEL, fg=ACCENT)
planet_subtitle.pack(anchor="w", padx=20, pady=(0, 14))
planet_badge = tk.Label(info_panel, text="TERRESTRIAL PLANET", font=("Arial", 9, "bold"), bg=PANEL2, fg=GOLD, padx=10, pady=7)
planet_badge.pack(anchor="w", padx=20, pady=(0, 16))

def info_row(parent, label):
    row = tk.Frame(parent, bg=PANEL2, padx=11, pady=9)
    row.pack(fill="x", padx=20, pady=4)
    tk.Label(row, text=label, font=("Arial", 9), bg=PANEL2, fg=MUTED).pack(anchor="w")
    value = tk.Label(row, text="", font=("Arial", 10, "bold"), bg=PANEL2, fg=WHITE, wraplength=245, justify="left")
    value.pack(anchor="w", pady=(3, 0))
    return value

distance_value = info_row(info_panel, "AVERAGE DISTANCE FROM SUN")
day_value = info_row(info_panel, "LENGTH OF A DAY")
year_value = info_row(info_panel, "LENGTH OF A YEAR")
moons_value = info_row(info_panel, "MOONS")
atmosphere_value = info_row(info_panel, "ATMOSPHERE")

tk.Label(info_panel, text="DID YOU KNOW?", font=("Arial", 10, "bold"), bg=PANEL, fg=GOLD).pack(anchor="w", padx=20, pady=(15, 5))
fact_label = tk.Label(info_panel, text="", font=("Arial", 10), bg=PANEL, fg=WHITE, wraplength=285, justify="left", anchor="nw")
fact_label.pack(fill="x", padx=20)

selected_planet = 2
planet_positions = {}

def draw_solar_system(event=None):
    canvas.delete("all")
    width = max(canvas.winfo_width(), 400)
    height = max(canvas.winfo_height(), 400)
    rng = random.Random(17)
    for _ in range(110):
        x = rng.randint(5, width - 5)
        y = rng.randint(5, height - 5)
        size = rng.choice([1, 1, 1, 2])
        canvas.create_oval(x, y, x + size, y + size, fill=rng.choice(["#ffffff", "#9db9ff", "#7f8db7"]), outline="")

    center_y = height // 2
    sun_x = 48
    sun_r = 20
    canvas.create_oval(sun_x - 29, center_y - 29, sun_x + 29, center_y + 29, fill="#30284a", outline="")
    canvas.create_oval(sun_x - sun_r, center_y - sun_r, sun_x + sun_r, center_y + sun_r, fill="#ffd166", outline="#fff0ad", width=2)
    canvas.create_text(sun_x, center_y + 39, text="SUN", fill="#ffd166", font=("Arial", 8, "bold"))

    start_x = 100
    end_x = width - 35
    step = (end_x - start_x) / 7 if end_x > start_x else 35
    planet_positions.clear()

    for index, planet in enumerate(planets):
        x = int(start_x + index * step)
        y = center_y + int(math.sin(index * 1.05) * min(48, height * 0.12))
        r = planet["radius"]
        if index == selected_planet:
            canvas.create_oval(x-r-7, y-r-7, x+r+7, y+r+7, outline=GOLD, width=2)
        if planet["name"] == "Saturn":
            canvas.create_oval(x-r-8, y-6, x+r+8, y+6, outline="#d6c28c", width=3)
        canvas.create_oval(x-r, y-r, x+r, y+r, fill=planet["color"], outline="#ffffff", width=1)
        if planet["name"] == "Earth":
            canvas.create_oval(x-5, y-5, x+2, y+1, fill="#4fd1a5", outline="")
        if planet["name"] == "Jupiter":
            canvas.create_line(x-r+3, y-3, x+r-3, y-3, fill="#b87d5e", width=2)
            canvas.create_line(x-r+4, y+4, x+r-4, y+4, fill="#b87d5e", width=2)
        canvas.create_text(x, y + r + 15, text=planet["name"], fill=WHITE if index == selected_planet else MUTED, font=("Arial", 8, "bold" if index == selected_planet else "normal"))
        planet_positions[index] = (x, y, r)

    canvas.create_text(width // 2, height - 22, text="Illustration only — planet sizes and distances are not to scale", fill="#7e8ba9", font=("Arial", 8))

def show_planet(index):
    global selected_planet
    selected_planet = index
    planet = planets[index]
    planet_title.config(text=planet["name"], fg=planet["color"])
    planet_subtitle.config(text=planet["order"])
    planet_badge.config(text=planet["type"].upper())
    distance_value.config(text=planet["distance"])
    day_value.config(text=planet["day"])
    year_value.config(text=planet["year"])
    moons_value.config(text=planet["moons"])
    atmosphere_value.config(text=planet["atmosphere"])
    fact_label.config(text=planet["fact"])
    draw_solar_system()

def on_canvas_click(event):
    nearest_index = None
    nearest_distance = None
    for index, (x, y, radius) in planet_positions.items():
        distance = math.sqrt((event.x - x) ** 2 + (event.y - y) ** 2)
        if distance <= radius + 10 and (nearest_distance is None or distance < nearest_distance):
            nearest_index = index
            nearest_distance = distance
    if nearest_index is not None:
        show_planet(nearest_index)

canvas.bind("<Button-1>", on_canvas_click)
canvas.bind("<Configure>", draw_solar_system)

tk.Label(info_panel, text="EXPLORE PLANETS", font=("Arial", 10, "bold"), bg=PANEL, fg=ACCENT).pack(anchor="w", padx=20, pady=(18, 8))
button_grid = tk.Frame(info_panel, bg=PANEL)
button_grid.pack(fill="x", padx=16)

for i, planet in enumerate(planets):
    button = tk.Button(button_grid, text=planet["name"], command=lambda index=i: show_planet(index),
                       font=("Arial", 9, "bold"), bg=PANEL2, fg=planet["color"],
                       activebackground="#293b60", activeforeground=WHITE,
                       relief="flat", cursor="hand2", padx=4, pady=7)
    button.grid(row=i // 2, column=i % 2, sticky="ew", padx=4, pady=4)

button_grid.columnconfigure(0, weight=1)
button_grid.columnconfigure(1, weight=1)

footer = tk.Label(root, text="MADE WITH PYTHON + TKINTER  •  Explore, click and learn", font=("Arial", 9, "bold"), bg=BG, fg="#7586ac")
footer.pack(side="bottom", pady=(0, 10))

root.after(150, lambda: show_planet(2))
root.mainloop()
