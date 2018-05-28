# 2048-Game
My recreation of the popular 2048 game in python
import gui
import random

"""
2048 Game
Please read the comments to help clear up any confusion about what the purpose of the various variables are

Before submitting ensure to edit this comment block to include the ID numbers of both group members
Group Member:620091991
Group Member:
"""

# Give grid the appropriate value

grid = [[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,0]]

# Values to represent the directions the tiles could move
UP = 1
DOWN = 2
RIGHT = 3
LEFT = 4

# Dictionary that might be useful
helper = {"count": 0, "Right": RIGHT, "Up": UP, "Left": LEFT, "Down": DOWN}

# Used to store the available keyboard controls
controls = ["<Right>", "<Left>", "<Up>", "<Down>"]

# used to help animate the movement from one tile to another, if functions are implemented correctly increasing
# this will increase the speed at which the tiles move and decreasing it will also cause the tiles to move slower
transition_value = 20


def empty_slots():
    elst=[]
    for (row,x) in enumerate(grid):
           for (column,value) in enumerate(x):
                if value==0:
                     elst=elst+[(row,column)]
    return elst
        
def random_position():
    return random.choice(empty_slots())


def find_identifier(game_board,row,column):
    for key,value in board.numbers.items():
        if value == (row, column):
            return key

        
def update_grid(row,column,direction):
    if direction==LEFT:
        if column-1>=0 and column-1<=3:
            if grid[row][column-1]==0:
                hold=grid[row][column]
                grid[row][column]=0
                grid[row][column-1]=hold
                return (row,column-1)
            elif grid[row][column-1]==grid[row][column]:
                grid[row][column-1]=grid[row][column-1]+grid[row][column]
                grid[row][column]=0
                return (row,column-1)
            else:
                return (row,column)
        else:
            return(row,column)
    elif direction==RIGHT:
        if column+1<=3:
            if grid[row][column+1]==0:
                hold=grid[row][column]
                grid[row][column]=0
                grid[row][column+1]=hold
                return (row,column+1)
            elif grid[row][column+1]==grid[row][column]:
                grid[row][column+1]=grid[row][column+1]+grid[row][column]
                grid[row][column]=0
                return(row,column+1)
            else:
                return(row,column)
        else:
             return(row,column)
            

    elif direction ==UP:
        if row-1>=0 and row-1<=3:
            
            if grid[row-1][column]==0:
                hold=grid[row][column]
                grid[row][column]=0
                grid[row-1][column]=hold
                return(row-1,column)
            elif grid[row-1][column]==grid[row][column]:
                grid[row-1][column]=grid[row-1][column]+grid[row][column]
                grid[row][column]=0
                return(row-1,column)
            else:
                return(row,column)
           
        else:
            return (row,column)
        
    elif direction==DOWN:
            if row+1<=3:
                if grid[row+1][column]==0:
                    count=4
                    hold=grid[row][column]
                    grid[row][column]=0
                    grid[row+1][column]=hold
                    return(row+1,column)
                
                elif grid[row+1][column]==grid[row][column]:
                    grid[row+1][column]=grid[row+1][column]+grid[row][column]
                    grid[row][column]=0
                    return(row+1,column)
                else:
                    return (row,column)
            else:
                return (row,column)
                                



def animate_movement(game_board, key, h_distance, v_distance, direction):
  
    def lessthantransitional(h_distance, v_distance):
        if direction is UP or direction is DOWN:
            return v_distance < transition_value
        elif direction is LEFT or direction is RIGHT:
            return h_distance < transition_value

    def amove(dist):
        if direction is UP:
            gui.move_tile(game_board, key, 0, -dist)
        elif direction is DOWN:
            gui.move_tile(game_board, key, 0, dist)
        elif direction is RIGHT:
            gui.move_tile(game_board, key, dist, 0)
        elif direction is LEFT:
            gui.move_tile(game_board, key, -dist, 0)


    def possiblemove(h_distance, v_distance):
        if h_distance is not 0 or v_distance is not 0:
            if lessthantransitional(h_distance, v_distance):
                if direction is UP or direction is DOWN:
                    amove(v_distance)
                    v_distance = 0
                else:
                    amove(h_distance)
                    h_distance = 0
                return True
            else:
                amove(transition_value)
                if direction is LEFT or direction is RIGHT:
                    return possiblemove(h_distance-transition_value,v_distance)
                else:
                    return possiblemove(h_distance,v_distance-transition_value)
        else:
            return False
    
    if possiblemove(h_distance, v_distance):
        if merge(game_board, key):
            return False
        else:
            return True
    else:
        return False




def add_random_number(game_board):
    key = "Tile number " +str(helper['count'])
    position= random_position()
    rnumber=gui.random_number()
    gui.put(game_board,key,rnumber,position[0],position[1])
    grid[position[0]][position[1]]=rnumber.value
    helper["count"]=helper["count"] + 1
    return (position[0],position[1])



def move(game_board,key,direction):
    tmove=gui.move_number(game_board,key,direction,update_grid,animate_movement)
    if tmove:
        return move(game_board,key,direction)
    else:
        return False
        
       

old_grid = [[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,0]]


def keyboard_callback(event, w_frame, game_board):
     for x, y in [(x,y) for x in range(4) for y in range(4)]:
          old_grid[x][y] = grid[x][y]
     unbind(w_frame, game_board)
     if is_game_over(game_board)==False:
          move_all(game_board, event)
          bind(w_frame, game_board)
          def gridtile():
               grid_lst=[]
               for y in range(4):
                    for x in range(4):
                         grid_lst=grid_lst+[(x,y)]
               return grid_lst
          gridlst=gridtile()
          for tile in gridlst:
               if old_grid[tile[0]][tile[1]] != grid[tile[0]][tile[1]]:
                    add_random_number(game_board)
                    break



def move_all_down(game_board):
     def gridtile():
          grid_lst=[]
          for y in range(4):
               for x in reversed(range(4)):
                    grid_lst=grid_lst+[(x,y)]
          return grid_lst
     gridlst=gridtile()
     for tile in gridlst:
          if tile in board.numbers.values():
               key=find_identifier(game_board, tile[0], tile[1])
               move(game_board, key, DOWN)

def move_all_up(game_board):
     def gridtile():
          grid_lst=[]
          for y in range(4):
               for x in range(4):
                    grid_lst=grid_lst+[(x,y)]
          return grid_lst
     gridlst=gridtile()
     for tile in gridlst:
          if tile in board.numbers.values():
               key=find_identifier(game_board, tile[0], tile[1])
               move(game_board, key, UP)

def move_all_right(game_board):
     def gridtile():
          grid_lst=[]
          for x in range(4):
               for y in reversed(range(4)):
                    grid_lst=grid_lst+[(x,y)]
          return grid_lst
     gridlst=gridtile()
     for tile in gridlst:
          if tile in board.numbers.values():
               key=find_identifier(game_board, tile[0], tile[1])
               move(game_board, key, RIGHT)

def move_all_left(game_board):
     def gridtile():
          grid_lst=[]
          for x in range(4):
               for y in range(4):
                    grid_lst=grid_lst+[(x,y)]
          return grid_lst
     gridlst=gridtile()
     for tile in gridlst:
          if tile in board.numbers.values():
               key=find_identifier(game_board, tile[0], tile[1])
               move(game_board, key, LEFT)

def move_all(game_board, event):
     if event.keysym == "Down":
          move_all_down(game_board)
     elif event.keysym == "Up":
          move_all_up(game_board)
     elif event.keysym == "Right":
          move_all_right(game_board)
     elif event.keysym == "Left":
          move_all_left(game_board)



def bind(w_frame,game_board):
    map(lambda string:frame.bind(string,lambda event:keyboard_callback(event,w_frame,game_board)),controls)
    
def unbind(w_frame,game_board):
    map(lambda string:frame.unbind(string),controls)

    
def merge(game_board,key):
    for x in board.numbers:
        if gui.find_position(game_board,key) == gui.find_position(game_board,x) and key != x:
            gui.remove_number(game_board,key)
            gui.remove_number(game_board,x)
            value=board.numbers.pop(key)
            board.numbers.pop(x)
            gamevalue=grid[value[0]][value[1]]
            gui.put(game_board,"Tile number" +str(helper["count"]),gui.find_number(gamevalue),value[0],value[1])
            helper["count"] =helper["count"]+1
            game_board.score=game_board.score+gamevalue
            gui.update_score(game_board)
            return True
    return False        
        

def is_game_over(game_board):
     def same_vslots():
          if grid[0][0]==grid[0][1]or grid[0][0]==grid[1][0]:
               return True
          elif grid[0][1]==grid[0][2]or grid[0][1]==grid[1][1]:
               return True
          elif grid[0][2]==grid[0][3]or grid[0][2]==grid[1][2]:
               return True
          elif grid[0][3]==grid[1][3]:
               return True
          elif grid[1][0]==grid[1][1]or grid[1][0]==grid[2][0]:
               return True
          elif grid[1][1]==grid[1][2]or grid[1][1]==grid[2][1]:
               return True
          elif grid[1][2]==grid[1][3]or grid[1][2]==grid[2][2]:
               return True
          elif grid[1][3]==grid[2][3]:
               return True
          elif grid[2][0]==grid[2][1]or grid[2][0]==grid[3][0]:
               return True
          elif grid[2][1]==grid[2][2]or grid[2][1]==grid[3][1]:
               return True
          elif grid[2][2]==grid[2][3]or grid[2][2]==grid[3][2]:
               return True
          elif grid[2][3]==grid[3][3]:
               return True
          elif grid[3][0]==grid[3][1]:
               return True
          elif grid[3][1]==grid[3][2]:
               return True
          elif grid[3][2]==grid[3][3]:
               return True
          return False 
          
     if same_vslots()==False and empty_slots() == []:
          gui.game_over(game_board,False)
          return True
     else:
          for row in grid:
               for val in row:
                    if val == 2048:
                         gui.game_over(game_board, True)
                         break
          else:
               return False




    
if __name__ == '__main__':
    """Your Program will start here"""

    frame, board = gui.setup()
   
           
    # Finishing setting up your GameBoard here, answer to Question 17 should go here

    row,column=add_random_number(board)
    row,column=add_random_number(board)
    bind(frame,board)
    
    gui.start(frame)






        

