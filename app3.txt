from flask import Flask,jsonify,request

playerinfo = [
    {
    'name':'Pradnya',
    'symbol':'P'
}, 
{
    'name':'Avinash',
    'symbol':'A'
}
]

PlayBoard = {  '1': ' ' , '2': ' ' , '3': ' ' ,
            '4': ' ' , '5': ' ' , '6': ' ' ,
            '7': ' ' , '8': ' ' , '9': ' ' }

WinnerName = { 'WinnerName': ' ' }

board_keys = []

for key in PlayBoard:
    board_keys.append(key)


def printBoard(board):
    print("" + str(board['1']) + "|" + str(board['2']) + "|" + str(board['3']) + "")
    print("" + '-+-+-' + "")
    print("" + board['4'] + '|' + board['5'] + '|' + board['6'] + "")
    print("" + '-+-+-' + "")
    print("" + board['7'] + '|' + board['8'] + '|' + board['9'] + "")

def game():
    
    turn = playerinfo[0]['symbol']
    count = 0


    for i in range(10):
        printBoard(PlayBoard)
        print("Move to which place?")

        move = input()        

        if PlayBoard[move] == ' ':
            PlayBoard[move] = turn
            count += 1
        else:
            print("That place is already filled.\nMove to which place?")
            continue

        
        if count >= 5:
            if PlayBoard['1'] == PlayBoard['2'] == PlayBoard['3'] != ' ': 
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" **** "  +turn + " WINNER: ****")                
                break
            elif PlayBoard['4'] == PlayBoard['5'] == PlayBoard['6'] != ' ':
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" **** "  +turn + " WINNER: ****")
                break
            elif PlayBoard['7'] == PlayBoard['8'] == PlayBoard['9'] != ' ': 
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" **** "  +turn + " WINNER: ****")
                break
            elif PlayBoard['1'] == PlayBoard['4'] == PlayBoard['7'] != ' ':
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" **** " +turn + " WINNER: ****")
                break
            elif PlayBoard['2'] == PlayBoard['5'] == PlayBoard['8'] != ' ': 
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" **** " +turn + " WINNER: ****")
                break
            elif PlayBoard['3'] == PlayBoard['6'] == PlayBoard['9'] != ' ': 
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" **** " +turn + " WINNER: ****")
                break 
            elif PlayBoard['7'] == PlayBoard['5'] == PlayBoard['3'] != ' ': 
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" **** "  +turn + "  WINNER: ****")
                break
            elif PlayBoard['1'] == PlayBoard['5'] == PlayBoard['9'] != ' ':
                printBoard(PlayBoard)
                print("\nGame Over.\n")                
                print(" ****   " +turn + " WINNER:  ****")
                break 

        
        if count == 9:
            print("\nGame Over.\n")                
            print("It's a Tie!!")

        if turn ==playerinfo[0]['symbol']:
            turn = playerinfo[1]['symbol']
        else:
            turn = playerinfo[0]['symbol']        
    
   


app = Flask(__name__)


# retrieve all playerinfo
@app.route('/players')
def get_allplayer():
    return jsonify({'players': playerinfo})

@app.route('/PlayBoard')
def get_allPlayBoard():
    return jsonify({'PlayBoard': PlayBoard})
   

# retrieve all one playerinfo.
@app.route('/player/<string:name>')
def get_oneplayerinfo(name):
  if not playerinfo or len(playerinfo) == 0:
      return jsonify({'message': 'does not have player information'})
  else:
    for player in playerinfo:
        if player['name'] == name:
            return jsonify({'message': 'WINNER not found'})

    return jsonify({'message': 'player not found'})


# create a new player .
@app.route('/player', methods=["POST"])
def player_input():
    request_data = request.get_json()
    playerinfo.append(request_data)
    return jsonify({"message": "Player has been added"})
    


@app.route("/play")
def playgame():
    printBoard(PlayBoard)
    display('1','A','Avinash',1)
    display('5','P','Pradnya',2)
    display('2','A','Avinash',3)
    display('4','P','Pradnya',4)
    display('3','A','Avinash',5)

    
    # display('1','A','Avinash',1)
    # display('3','P','Pradnya',2)
    # display('2','A','Avinash',3)
    # display('4','P','Pradnya',4)
    # display('5','A','Avinash',5)
    # display('8','P','Avinash',6)
    # display('6','A','Avinash',7)
    # display('9','P','Avinash',8)
    # display('7','A','Avinash',9)
    if WinnerName['WinnerName'] == 'Nowinner':
            return jsonify({"message": "It's a Tie!!"})   
    else:   
            return jsonify({"message": " **** "  + WinnerName['WinnerName'] + " is WINNER: ****"})

def display(move,symbol,playername,count):
        if PlayBoard[move] == ' ':
            PlayBoard[move] = symbol 
        
        if count >= 5:
            if PlayBoard['1'] == PlayBoard['2'] == PlayBoard['3'] != ' ': 
                WinnerName['WinnerName'] = playername
            elif PlayBoard['4'] == PlayBoard['5'] == PlayBoard['6'] != ' ':
                WinnerName['WinnerName'] = playername
            elif PlayBoard['7'] == PlayBoard['8'] == PlayBoard['9'] != ' ': 
                WinnerName['WinnerName'] = playername
            elif PlayBoard['1'] == PlayBoard['4'] == PlayBoard['7'] != ' ':
                WinnerName['WinnerName'] = playername
            elif PlayBoard['2'] == PlayBoard['5'] == PlayBoard['8'] != ' ': 
                WinnerName['WinnerName'] = playername
            elif PlayBoard['3'] == PlayBoard['6'] == PlayBoard['9'] != ' ': 
               WinnerName['WinnerName'] = playername
            elif PlayBoard['7'] == PlayBoard['5'] == PlayBoard['3'] != ' ': 
                WinnerName['WinnerName'] = playername
            elif PlayBoard['1'] == PlayBoard['5'] == PlayBoard['9'] != ' ':
                WinnerName['WinnerName'] = playername

        if count == 9:
            WinnerName['WinnerName'] = 'Nowinner' 

if __name__ == "__main__":
  game()
  app.run(debug=True, port=5000)