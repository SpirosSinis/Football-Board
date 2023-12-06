# Visualizing a Football Board in Python

We present a simple Python code aiming to visualize a football board. The code offers the following features:

- Addition of 22 players on a 2D football pitch.
- Inclusion of player names.
- Automatic color change from red to blue.

The code utilizes the `mplsoccer` library to create the football pitch and display the players. Additionally, there is an option to add names to each player, with an automatic color change from red to blue after adding 11 players per team.

See the code below for a straightforward and flexible implementation.

```python
from mplsoccer import Pitch
import matplotlib.pyplot as plt
from tkinter.simpledialog import askstring

class FootballBoard:
    def __init__(self):
        self.pitch = Pitch(pitch_color="grass", line_color="white", stripe=True)
        self.fig, self.ax = self.pitch.draw(figsize=(10, 5))
        self.players = []

        # Add event handler for clicks
        self.fig.canvas.mpl_connect('button_press_event', self.on_click)

    def get_player_name(self, player_number):
        # Use simpledialog to prompt for player name
        return askstring("Player Name", f"Enter the name of Player {player_number}")

    def on_click(self, event):
        color = 'red' if len(self.players) < 11 else 'blue'
        player_position = (event.xdata, event.ydata)
        player = self.ax.scatter(*player_position, c=color, edgecolors='black', s=100, marker='o')
        self.players.append(player)

        # Automatic color change after adding 11 players per team
        if len(self.players) == 11:
            plt.title("You added 11 players. Now add players for the second team.")
            plt.draw()

    def show(self):
        plt.show()

if __name__ == "__main__":
    football_board = FootballBoard()
    plt.show()
