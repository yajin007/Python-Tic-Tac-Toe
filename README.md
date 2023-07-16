# Python-Tic-Tac-Toe
# Python-Tic-Tac-Toe
tic tac toe game in python
def printBoard(board):
    print("Current State of the Board:\n")
    for i in range(0, 9):
        if i > 0 and i % 3 == 0:
            print("\n")
        if board[i] == 0:
            print("   ", end=" ")
        elif board[i] == -1:
            print(" X ", end=" ")
        elif board[i] == 1:
            print(" O ", end=" ")
    print("\n")

def User1Turn(board):
    pos = int(input("Enter X's position from [1, 2, 3, ..., 9]: "))
    if board[pos - 1] != 0:
        print("Wrong Move")
        exit(0)
    board[pos - 1] = -1

def User2Turn(board):
    pos = int(input("Enter O's position from [1, 2, 3, ..., 9]: "))
    if board[pos - 1] != 0:
        print("Wrong Move")
        exit(0)
    board[pos - 1] = 1

def analyzeBoard(board):
    cb = [[0, 1, 2], [3, 4, 5], [6, 7, 8], [0, 3, 6], [1, 4, 7], [2, 5, 8], [0, 4, 8], [2, 4, 6]]
    for i in range(0, 8):
        if (
            board[cb[i][0]] != 0
            and board[cb[i][0]] == board[cb[i][1]]
            and board[cb[i][0]] == board[cb[i][2]]
        ):
            return board[cb[i][0]]
    return 0

def minmax(board, player):
    x = analyzeBoard(board)
    if x != 0:
        return x * player

    pos = -1
    value = -2
    for i in range(0, 9):
        if board[i] == 0:
            board[i] = player
            score = -minmax(board, player * -1)
            board[i] = 0
            if score > value:
                value = score
                pos = i

    if pos == -1:
        return 0
    return value

def CompTurn(board):
    pos = -1
    value = -2
    for i in range(0, 9):
        if board[i] == 0:
            board[i] = 1
            score = -minmax(board, -1)
            board[i] = 0
            if score > value:
                value = score
                pos = i
    board[pos] = 1

def main():
    choice = int(input("Enter 1 for Single Player, 2 for Multiplayer: "))
    board = [0] * 9

    if choice == 1:
        print("Computer: O Vs. You: X")
        player = int(input("Enter to play 1 (st) or 2 (nd): "))

        for i in range(0, 9):
            if analyzeBoard(board) != 0:
                break

            if (i + player) % 2 == 0:
                CompTurn(board)
            else:
                printBoard(board)
                User1Turn(board)
    else:
        for i in range(0, 9):
            if analyzeBoard(board) != 0:
                break

            if i % 2 == 0:
                printBoard(board)
                User1Turn(board)
            else:
                printBoard(board)
                User2Turn(board)

    result = analyzeBoard(board)
    printBoard(board)

    if result == 0:
        print("Draw!")
    elif result == -1:
        print("Player X Wins! Player O Loses!")
    elif result == 1:
        print("Player O Wins! Player X Loses!")

main()

