

# tmux commands

|Command	|	Description|
|:---------:|:------------:|
|`tmux`							|start tmux|
|`tmux new -s <name>`				|start tmux with <name>|
|`tmux ls`						|shows the list of sessions|
|`tmux a #`						|attach the detached-session|
|`tmux a -t <name>`				|attach the detached-session to <name>|
|`tmux kill-session –t <name>`	|kill the session <name>|
|`tmux kill-server`				|kill the tmux server|

- Heirarchy is like:
  - session
  - window
  - panes(split windows horizontally, vertically multiple times)

### Sessions
```
ctrl-b :new		create new session
s				list sessions
$				rename sessions
d				detach session
```


### Windows
```
w			list windows and select onein
,			rename window
c or N		create new window
&			kill window
0-9			go to window 0-9
```



``` bash
C-b 0       Select window 0                                     
C-b 1       Select window 1                                                              
C-b 2       Select window 2                                         
C-b 3       Select window 3                                     
C-b 4       Select window 4                                        
C-b 5       Select window 5                                      
C-b 6       Select window 6                                       
C-b 7       Select window 7                                       
C-b 8       Select window 8                                       
C-b 9       Select window 9
```


### Panes (split windows)
```
C-b "       Split window vertically                                                      
C-b %       Split window horizontally
```


```
C-b Up      Select the pane above the active pane
C-b Down    Select the pane below the active pane
C-b Left    Select the pane to the left of the active pane
C-b Right   Select the pane to the right of the active pane
```


```
C-b x				kill pane
C-b o				go to next pane
C-b h,j,k,l			go to next pane in vim-style
C-b z				toggle full-screen mode for current pane
C-b arrow keys		resize the pane
C-b q				show pane-numbers
```


```
C-b #       List all paste buffers                              
C-b $       Rename current session                               
C-b &       Kill current window
```
