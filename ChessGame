from os import system

""" Game Rules
1) White moves first.
2) Pawns can move forward one space to an empty location, or diagonally one space to capture an
opponent’s pawn at that location.
3) A player must capture an opponent’s pawn if a capturing move is available.
4) On any capturing move, the player immediately moves again with another pawn that has not yet moved
that turn.
5) The first player to get a pawn to the opposite end of the board wins, unless a player is left without any
move, in which case the other player immediately wins.
6) Black pawns start at row 1 and and can only move towards row 7, white pawns start at row 6 and can only move towards row 0

"""

class EverChess(object):

  def __init__(self):

    self.board = []
    self.blackpawns = [None]*8
    self.whitepawns = [None]*8
    self.whiteplayed = [False]*8
    self.blackplayed = [False]*8
    self.initializeBoard()

  def initializeBoard(self):

    for i in range(8):
      self.board.append(["__"]*8)

    for i in range(8):
      self.board[1][i] = "B" + str(i)
      self.board[6][i] = "W" + str(i)

      self.blackpawns[i] = [1,i]
      self.whitepawns[i] = [6,i]
      

  def playGame(self):

    black = False

    while(True):

      self.displayState()

      if(not self.checkIfMovePossible(black)):
        if(black):
          print("Player white wins the game")
        
        else:
          print("Player black wins the game")
        
        break

      if(black):
        print("Player Black Turn")
      
      else:
        print("Player White Turn")

      errStatus = True

      [pawni,pawnj,pawntargeti,pawntargetj] = [0,0,0,0]
      
      while(errStatus):

        print("Enter the coordinates in the form of sourcerow <space> sourcecol <space> targetrow <space> targetcol")

        [pawni,pawnj,pawntargeti,pawntargetj] = input().strip().split(" ")
        pawni = int(pawni)
        pawnj = int(pawnj)
        pawntargeti = int(pawntargeti)
        pawntargetj = int(pawntargetj)

        [errStatus, errMessage] = self.validateInput(pawni,pawnj,pawntargeti,pawntargetj,black)
      
        if(errStatus):
          print(errMessage)

      captured = False
      
      if(black):
        pawnnumber = int(self.board[pawni][pawnj][1])

        self.blackpawns[pawnnumber] = [pawntargeti,pawntargetj]
        self.blackplayed[pawnnumber] = True

        if(self.board[pawntargeti][pawntargetj] != "__"):
          self.whitepawns[int(self.board[pawntargeti][pawntargetj][1])] = [-1,-1]
          captured = True

        self.board[pawntargeti][pawntargetj] = self.board[pawni][pawnj]
        self.board[pawni][pawnj] = "__"
      
      else:
        pawnnumber = int(self.board[pawni][pawnj][1])

        self.whitepawns[pawnnumber] = [pawntargeti,pawntargetj]
        self.whiteplayed[pawnnumber] = True

        if(self.board[pawntargeti][pawntargetj] != "__"):
          self.blackpawns[int(self.board[pawntargeti][pawntargetj][1])] = [8,8]
          captured = True

        self.board[pawntargeti][pawntargetj] = self.board[pawni][pawnj]
        self.board[pawni][pawnj] = "__"
      
      if(not captured):
        black = not black
        self.whiteplayed = [False]*8
        self.blackplayed = [False]*8

      if(self.checkIfGameEnds()):
        break

    

      system("clear")

  def validateInput(self, pawni, pawnj, pawntargeti, pawntargetj, black):

    ## 1) check if the player has a capturing move 
    ## 2) if yes check if the move is one of the capturing move
    ## 3) check if player has a pawn in the indicated position
    ## 4) check if the move is just one position ahead and there is no pawn ahead of it

    ## 5) 

    if(pawni >= 8 or pawnj >= 8 or pawntargeti >= 8 or pawntargetj >= 8 or pawni < 0 or pawnj < 0 or pawntargeti < 0 or pawntargetj < 0):
      return [True, "Dimensions must be valid"]

    if((black and pawni + 1 != pawntargeti) or (not black and pawni - 1 != pawntargeti)):
      return [True, "Player must move his pawn ahead always"]
    
    if(self.board[pawni][pawnj] != "__"):

      if(black and self.board[pawni][pawnj][0] != "B"):
        return [True, "No Black pawn exists at the indicated position"]
      
      if(not black and self.board[pawni][pawnj][0] != "W"):
        return [True, "No White pawn exists at the indicated position"]

      if(black):

        if(not (self.board[pawni][pawnj][0] == "B" and not self.blackplayed[int(self.board[pawni][pawnj][1])])):
          return [True,"Cannot make the move"]
      
      else:
      
        if(not (self.board[pawni][pawnj][0] == "W" and not self.whiteplayed[int(self.board[pawni][pawnj][1])])):
          return [True,"Cannot make the move"]
        
    
    else:
      return [True, "No pawn exists in the indicated source position"]

    if(self.checkIfCapturePossible(black)):

      if((black and (pawnj +1 == pawntargetj or pawnj - 1 == pawntargetj) and self.board[pawni][pawnj][0] == "B" and self.board[pawntargeti][pawntargetj][0] == "W") or (not black and (pawnj +1 == pawntargetj or pawnj - 1 == pawntargetj) and self.board[pawni][pawnj][0] == "W" and self.board[pawntargeti][pawntargetj][0] == "B")):

        return [False,""]
      
      return [True, "Capturing move must be played"]

    else:

      if((black and pawntargeti == pawni +1 and pawntargetj == pawnj and self.board[pawntargeti][pawntargetj] == "__") or (not black and pawntargeti == pawni - 1 and pawntargetj == pawnj and self.board[pawntargeti][pawntargetj] == "__")):

        return [False,""]

      return [True, "No Pawn exists at the source position or cannot move the pawn ahead"]





  def checkIfCapturePossible(self, black):

    if(black):

      for i in range(8):
        if(not self.blackplayed[i] and ((self.blackpawns[i][0] + 1 < 8 and self.blackpawns[i][1] + 1 < 8 and self.board[self.blackpawns[i][0] + 1][self.blackpawns[i][1] + 1] != "__" and self.board[self.blackpawns[i][0] + 1][self.blackpawns[i][1] + 1][0] == "W") or (self.blackpawns[i][0] + 1 < 8 and self.blackpawns[i][1] - 1 >= 0 and self.board[self.blackpawns[i][0] + 1][self.blackpawns[i][1] - 1] != "__" and self.board[self.blackpawns[i][0] + 1][self.blackpawns[i][1] - 1][0] == "W"))):
          return True
    
    else:

      for i in range(8):
        if(not self.whiteplayed[i] and ((self.whitepawns[i][0] - 1 >= 0 and self.whitepawns[i][1] + 1 < 8 and self.board[self.whitepawns[i][0] - 1][self.whitepawns[i][1] + 1] != "__" and self.board[self.whitepawns[i][0] - 1][self.whitepawns[i][1] + 1][0] == "B") or (self.whitepawns[i][0] - 1 >= 0 and self.whitepawns[i][1] - 1 >= 0 and self.board[self.whitepawns[i][0] - 1][self.whitepawns[i][1] - 1] != "__" and self.board[self.whitepawns[i][0] - 1][self.whitepawns[i][1] - 1][0] == "B"))):
          return True

    return False
  
  def checkIfGameEnds(self):

    for i in range(8):

      if(self.board[0][i] != "__"):   
        print("Player White Wins!")
      
        return True

      if(self.board[7][i] != "__"):

        print("Player Black Wins!")

        return True
    
    return False
  
  def checkIfMovePossible(self, black):

    if(self.checkIfCapturePossible(black)):
      return True

    if(black):

      for i in range(8):
        if(not self.blackplayed[i] and self.blackpawns[i][0] + 1 < 8 and self.board[self.blackpawns[i][0] + 1][self.blackpawns[i][1]] == "__"):
          return True

    else:

      for i in range(8):
        if(not self.whiteplayed[i] and self.whitepawns[i][0] - 1 >= 0 and self.board[self.whitepawns[i][0] - 1][self.whitepawns[i][1]] == "__"):
          return True
    
    return False
  
  def displayState(self):

    for i in range(8):
      for j in range(8):
        print(self.board[i][j], end = "  ")
      
      print()
      print()



if  __name__  ==  "__main__":
    
    chessgame = EverChess()
    chessgame.playGame()


  
