# Hamdan Kamal Fahrudin (5311421084)
# Tujuan
Meningkatkan pemahaman mahasiswa terkait dengan kode permainan tic tac toe. Selain itu, modul 6 menyediakan pengetahuan mengenai pemrograman berorientasi objek dengan menggunakan salah satu bahasa pemrograman, seperti java, python, atau C.
# Landasan Teori
Penyelesaian masalah permainan tic tac toe dapat menggunakan algoritma heuristic untuk mencapai solusi yang optimal. Pada modul ini memperlihatkan bagaimana membuat sebuah permainan tic tac toe. Initial state dari permainan ini adalah puzzle ukuran 8 yang tidak berisikan apaapa. Ketika pemain pertama menekan salah satu ubin, maka ubin tersebut akan diberikan tanda silang. Pemain kedua harus menghalangi pemain pertama untuk membuat tanda silang berjajaran baik secara vertikal, horizontal, atau diagonal. Permainan ini akan berakhir (goal state) ketika salah seorang pemain sudah menderetkan tanda meraka masing-masing baik secara vertikal, horizontal, atau diagonal. Solusi dari permasalahan ini dapat dilakukan dengan membuat topologi Tree, kemudian setiap langkah dari pemain pertama atau kedua akan menjadikan initial state selanjutnya, kemudian langkah tersebut akan dijadikan sebagai initial state yang baru sampai menemukan goal statenya. Ilustrasi penyelesaian masalah permainan tic tac toe ini dapat dilihat melalui Gambar berikut.
![image](https://github.com/hamdankf/Tugas-Best-first-search/assets/149086558/54356c2d-64d1-435e-92a5-e908c5e6f17d)
# Code
import tkinter as tk
from tkinter import messagebox

class TicTacToeGUI:
    def __init__(self):
        self.frame = tk.Tk()
        self.frame.title("Tic Tac Toe")
        self.frame.geometry("300x380")

        self.buttons = [[None for _ in range(3)] for _ in range(3)]
        self.current_player = "X"

        self.turn_label = tk.Label(self.frame, text=f"Player {self.current_player}'s turn", font=("Arial", 12))
        self.turn_label.grid(row=0, columnspan=3)

        self.create_board()
        self.create_reset_button()

    def create_board(self):
        for i in range(3):
            for j in range(3):
                self.buttons[i][j] = tk.Button(self.frame, text="", font=("Arial", 20), width=5, height=2, command=lambda i=i, j=j: self.on_click(i, j))
                self.buttons[i][j].grid(row=i + 1, column=j)

    def create_reset_button(self):
        reset_button = tk.Button(self.frame, text="Reset", font=("Arial", 16), command=self.reset_board)
        reset_button.grid(row=4, columnspan=3)

    def reset_board(self):
        for i in range(3):
            for j in range(3):
                self.buttons[i][j]["text"] = ""
                self.buttons[i][j]["state"] = "normal"
        self.current_player = "X"
        self.update_turn_label()

    def on_click(self, row, col):
        if self.buttons[row][col]["text"] == "":
            self.buttons[row][col]["text"] = self.current_player
            self.buttons[row][col]["state"] = "disabled"
            if self.check_winner():
                messagebox.showinfo("Game Over", f"Player {self.current_player} Wins!")
                self.reset_board()
            elif self.is_board_full():
                messagebox.showinfo("Game Over", "It's a Tie!")
                self.reset_board()
            else:
                self.switch_player()

    def switch_player(self):
        self.current_player = "O" if self.current_player == "X" else "X"
        self.update_turn_label()

    def update_turn_label(self):
        self.turn_label["text"] = f"Player {self.current_player}'s turn"

    def check_winner(self):
        for i in range(3):
            if all(self.buttons[i][j]["text"] == self.current_player for j in range(3)) or all(self.buttons[j][i]["text"] == self.current_player for j in range(3)):
                return True

        if all(self.buttons[i][i]["text"] == self.current_player for i in range(3)) or all(self.buttons[i][2 - i]["text"] == self.current_player for i in range(3)):
            return True

        return False

    def is_board_full(self):
        return all(self.buttons[i][j]["text"] != "" for i in range(3) for j in range(3))

    def start_game(self):
        self.frame.mainloop()

if __name__ == "__main__":
    game_gui = TicTacToeGUI()
    game_gui.start_game()

# Output
![image](https://github.com/hamdankf/Tugas-Best-first-search/assets/149086558/225c216d-2dd5-4e91-b881-938af566ed91)
![image](https://github.com/hamdankf/Tugas-Best-first-search/assets/149086558/8b1424d2-46cc-4182-9c05-9e6a8e77fb28)

# Penjelasan Code
Dalam tugass kali ini saya menggunakan bahasa pemograman python

import tkinter as tk
from tkinter import messagebox

Dalam kode ini, terdapat penggunaan modul tkinter untuk membuat antarmuka pengguna grafis (GUI) dan modul messagebox untuk menampilkan kotak pesan di dalam GUI.

class TicTacToeGUI:

kelas utama yang mengatur GUI permainan.

def __init__(self):
        self.frame = tk.Tk()
        self.frame.title("Tic Tac Toe")
        self.frame.geometry("300x380")

        self.buttons = [[None for _ in range(3)] for _ in range(3)]
        self.current_player = "X"

        self.turn_label = tk.Label(self.frame, text=f"Player {self.current_player}'s turn", font=("Arial", 12))
        self.turn_label.grid(row=0, columnspan=3)

__init__(self) merupakan konstruktor untuk menginisialisasi objek kelas. Di dalamnya, kita membuat jendela tkinter (self.frame), tombol-tombol (self.buttons), dan label untuk menunjukkan giliran pemain (self.turn_label). Selain itu, kita menginisialisasi giliran pertama sebagai "X".

self.create_board()
        self.create_reset_button()

    def create_board(self):
        for i in range(3):
            for j in range(3):
                self.buttons[i][j] = tk.Button(self.frame, text="", font=("Arial", 20), width=5, height=2, command=lambda i=i, j=j: self.on_click(i, j))
                self.buttons[i][j].grid(row=i + 1, column=j)

create_board(self) membuat papan permainan dengan 3x3 tombol menggunakan nested loop. Setiap tombol dipasang command self.on_click(i, j), yang berarti saat tombol diklik, akan memanggil metode on_click dengan parameter baris (i) dan kolom (j).

def create_reset_button(self):
        reset_button = tk.Button(self.frame, text="Reset", font=("Arial", 16), command=self.reset_board)
        reset_button.grid(row=4, columnspan=3)

create_reset_button(self) membuat tombol reset yang memanggil metode reset_board saat ditekan.

def reset_board(self):
        for i in range(3):
            for j in range(3):
                self.buttons[i][j]["text"] = ""
                self.buttons[i][j]["state"] = "normal"
        self.current_player = "X"
        self.update_turn_label()

reset_board(self) mengatur ulang papan permainan dan mengganti giliran pemain menjadi "X".

def on_click(self, row, col):
        if self.buttons[row][col]["text"] == "":
            self.buttons[row][col]["text"] = self.current_player
            self.buttons[row][col]["state"] = "disabled"
            if self.check_winner():
                messagebox.showinfo("Game Over", f"Player {self.current_player} Wins!")
                self.reset_board()
            elif self.is_board_full():
                messagebox.showinfo("Game Over", "It's a Tie!")
                self.reset_board()
            else:
                self.switch_player()

on_click(self, row, col) dipanggil saat seorang pemain mengklik tombol pada baris row dan kolom col. Jika tombol tersebut kosong, maka isinya diubah sesuai giliran pemain ("X" atau "O"). Kemudian, dilakukan pengecekan apakah ada pemenang atau seri, dan giliran pemain diubah.

def switch_player(self):
        self.current_player = "O" if self.current_player == "X" else "X"
        self.update_turn_label()

switch_player(self) mengganti giliran pemain dari "X" ke "O" atau sebaliknya.

def update_turn_label(self):
        self.turn_label["text"] = f"Player {self.current_player}'s turn"

pdate_turn_label(self) mengupdate label yang menunjukkan giliran pemain saat ini.

def check_winner(self):
        for i in range(3):
            if all(self.buttons[i][j]["text"] == self.current_player for j in range(3)) or all(self.buttons[j][i]["text"] == self.current_player for j in range(3)):
                return True

        if all(self.buttons[i][i]["text"] == self.current_player for i in range(3)) or all(self.buttons[i][2 - i]["text"] == self.current_player for i in range(3)):
            return True

        return False

check_winner(self) memeriksa apakah ada pemain yang menang dengan melihat baris, kolom, dan diagonal.

def is_board_full(self):
        return all(self.buttons[i][j]["text"] != "" for i in range(3) for j in range(3))

is_board_full(self) memeriksa apakah papan permainan sudah penuh (seri).

def start_game(self):
        self.frame.mainloop()

if __name__ == "__main__":
    game_gui = TicTacToeGUI()
    game_gui.start_game()

start_game(self) memulai permainan dengan menampilkan jendela tkinter.
Di bagian __name__ == "__main__":, objek TicTacToeGUI dibuat dan permainan dimulai dengan memanggil metode start_game(). Saat permainan dimulai, jendela tkinter akan ditampilkan, dan pemain dapat mulai mengklik tombol untuk bermain.
