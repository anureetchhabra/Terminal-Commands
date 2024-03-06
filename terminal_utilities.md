# Terminal Utilities

**************************************
## Learn Bash
**************************************
1. **for loop**: `for FILE in *;do tar -xzf $FILE; done` this command will extract each .tgz file in current directory.
    Types of for loop in bash
    ``` bash
    for i in 1 2 3 4 5
    for i in {1..5}
    for i in {0..10..2}
    # sorted_files is an array given to you.
    for (( i=0; i<100; i++ )); do
      file=${sorted_files[$i]}
      rm "$file"
      echo "Deleted $file"
    done
    for file in *;do echo $file $(ls $file/rgb|wc -l) $(cat $file/groundtruth.txt|wc -l); d
    ```
    *To calculate the sum of all files inside folders in current directory.*find
    - Note: $(( )) is used to write RHS of an expression[to do sum, subtract, etc]
    ``` bash
    sum=0
    for dir in *; do sum=$(($sum+$(ls $dir/image_left | wc -l)));done   #$(ls $dir/image_left | wc -l) :used for output of a command 
    #or
    for dir in */*;do sum=$(($sum+1)); done
    for dir in *; do 
      curr=$(ls $dir/image_left | wc -l);
      sum=$(($sum+$curr));done
    echo $sum
    ```
    *Because to add two variables in bash we have to write RHS inside $(( ))*
    ``` bash 
    var=0
    for i in {1..5};do var=$(( $var + 1 )); done
    #or
    for i in {1..5};do var=$(( $var + $sum )); done
    ```
    *To unzip TartanAir datset using for loop:*
    ``` bash
    #(just to do it parallely ... done Easy and Hard in 2 terminals... otherwise could have done using just * )
    for file in */Easy/*.zip;do 7z x $file -o./TartanAir_extracted;done
    for file in */Hard/*.zip;do 7z x $file -o./TartanAir_extracted;done
    ```
**************************************
2. **Arrays in bash**:
eg: 
``` bash
arr=("cat" "dog" "mouse" "frog")   # no space around "="... no comma to separate element of array... 
for str in ${arr[@]}; do
  echo $str
done

a=(1 2 3 "abc")
# outputs are in comments
echo $a  # 1
echo ${a} # 1
#to print all elements @ is used
#The @ symbol in the square brackets indicates that you are looping through all of the elements in the array. 
echo ${a[@]} # 1 2 3 abc 
echo $a[@]   # 1[@]  
# so ${} is necassary to access arrays to consider a and [@] together. If you were to leave that out and just write for str in ${myArray}, only the first string in the array would be printed.
# but variables don't need curly brackets, they can be directly used like @var1


#Loop Through Array Indices [ use ! at front]
for i in ${!arr[@]}; do  
  echo "element $i is ${arr[$i]}"
done
```
******************************************************
**printf in bash**:
The syntax for the printf command is as follows: `printf [-v var] format [arguments]`

ex:   
- `printf "Name: %s, Age: %d \n" "Suraj" 24`  // output: Name: Suraj, Age: 24
- `printf "Open issues: %s\nClosed issues: %s\n" "34" "65"`  
    ```
    #output:  
    Open issues: 34
    Closed issues: 65
    ```
- ``` bash
    for file in */rgb;do 
      printf "%20s %d\n" "${file%%/rgb}" $(ls $file|wc -l);
    done;`

- `printf "%05d" 10` // output: 00010
- When an asterisk (*) is used as a width directive, then the width of the conversion field is set by a width argument that precede the argument that’s being formatted.
  `printf "%0*d" 10 5` // output: 0000000005
- `printf "%.3f" 1.61803398` // output: 1.618
- `printf "%.*f" 3 1.61803398` // output: 1.618
******************************************************

**iPython Debugger:**:

- write below line where you want to pause:
ipdb: `import ipdb; ipdb.set_trace()` (for objects you can press Tab after . here to see it's variables)
pdb : `import pdb; pdb.set_trace()`

- Once the program pauses at the breakpoint, you can use the following commands to step through your code:  
  
  *I have also written in italics what is equivalent to in vscode*  
    - *step-over (F10)*:    `n` or next: execute the current line and move to the next line, without stepping into any function calls  
    - *step-into (F11)*:    `s` or step: execute the current line and step into any function calls on that line  
    - *NIL*:                `r` : return ...Continue execution until the current function returns (end of the function).  
    - *step-out (Shit+F11)*:`r` then `n` :  from current function call  
    - *continue (F5)*:      `c` or continue:  execution until the next breakpoint or the end of the program is reached  
    - *quit (Shift+F5)*:    `q` or quit:  pdb and stop execution of the program  


- You can also use the `l` command to list the code surrounding the current line, and `p` command to print the value of a variable at the current execution point. eg:
    p var1
    p var2

******************************************************

- **find**: 
  - `find ~/Downloads  -maxdepth 2 -name "*.txt" -empty -perm 664 -type f`
  *use -iname for ignore case:*
  - `find ~/Downloads -iname "nyu*.txt" -maxdepth 2 -empty -perm 664 -type f -exec grep 'Geek' {} \;` 
  - `find */* -type d`: to list all the subdirectories after 1st level (all subdirectories excluding directories in  current directory)
  - `find ./ -name "*.txt" -maxdepth 2 -type f -exec grep 'Geek' {} \;` (ans of *How to grep with maxdepth*)
  - to remove all the directories in the current directory: `find -type d -exec rm -rf {} \;`
  - to exclude/ignore some directory(use `-not -path expression`): `find -type d -not -path "./dir1/*" `
    then you can remove them : `find -type d -not -path "./dir1/*" -exec rm -rf {} \;`
  - `rsync -avr -e "ssh -l suraj" --exclude 'weights*' ./ vision04.cse.iitd.ac.in:~/scratch/karan/new_from_vision02`
    <br>this path should exist already from vision04's home: `./scratch/karan/new_from_vision02`



**************************************

- **pdfgrep**: `pdfgrep -inr ./ "bundle adjustment"` ((-i ignore case) (-n line numbers in file) (-r recursively search in the dir given))
**************************************

- **unzip**: to unzip all files in curr dir: `unzip \*.zip` `unzip abc.zip -d ./target_dir`
    to unzip all the files in current dir in their respective folders: 
    `for FILE in *.zip; do unzip $FILE -d "${FILE%%.zip}"; done` 
    or 000
    `find ./ -name '*.zip' -exec sh -c 'unzip "$1" -d "${1%.*}"' _ {} \;`
    ``` bash
      ~% FILE="example.tar.gz"

      ~% echo "${FILE%%.*}"
      example

      ~% echo "${FILE%.*}"
      example.tar

      ~% echo "${FILE#*.}"
      tar.gz

      ~% echo "${FILE##*.}"
      gz
    ```
**************************************

- **tar**: `tar -xzf abc.tar -C /path/to/directory` ((-x for xtract) (z for using GNUzip) (f for mentioning file name at last)). see below command for extracting all files in current directory.
**************************************

- **search in man pages**: `press / in a man page and type any regular expression`
- **search for a string "edit files" in all man pages**: `man -wK "edit files"`

**************************************
- **Regular Expressions**: 
    ``` python
    ?    for zero or one of the preceding expression
    *    for zero or more of the preceding expression
    +    for one or more of the preceding expression
    ```
  - expressions can be "grouped" with parentheses, as in (--)+ (for two or more dashes).
  - [abAB1]* : starting with 1 or a or b (or in capital)
  - [a-zA-Z0-9]* : starting with letters or digits.
  - (--)[^a-z] : all long arguments not starting with small letters.
  - color|colour : which finds every occurrence of either "color" or "colour" (this is sometimes called boolean OR).
**************************************

- **All about PATH in Ubuntu**: 
  - $PATH is an `:` seperated environment variable which gets loaded from `etc/environment` file(for all users). and as we login `.profile` gets executed, it might add append something to it for the current user's login.(.profile of current user(spatni)) gets executed. And as we open a bash terminal `.bashrc` gets executed, it might also append something to path for the current shell. same for zsh terminal , `.zshrc` gets executed.
    - first of these will work:
      ``` bash
      ~/.bash_profile
      ~/.bash_login
      ~/.profile
      ```
  - How to append(add something to) PATH : `export PATH="/path/to/your/dir/where/have/your/executable:$PATH"`
  - If u execute this line to current terminal, then PATH gets appended for only current login, If you add this to `.bashrc`, then it gets appended whenever u open a new bash terminal from now. If you add this line to `.profile`, then it gets appended whenever u login as the current user(which current that `.profile` file.
**************************************


- **`pip show opencv-python`** : to show all the details (**version**, **install_location**, Summary, Required packages, Author,etc) about a package. 
- start ssh server at your machine: `sudo apt-get install openssh-server` then check status as active using: `systemctl status ssh`
**************************************


- **du**:`du -sh *` <-- this is preferable(shows for dir and files both).
  - `du -h ./ --max-depth=1` : to list size of all dirs in current dir in human-readable form.
  - `du -hs` to print total size of current directory.
- **df**: `df -h` used to show disk partitions size(total, used and available) in human-readable form with the type of partition(/,/home,...etc)
**************************************

- **tree**: `tree ./ -L 1 -d` here `-d` is used to show only directories. `-L` is used to show till level 1
  ` tree ./ -I '*.npy|*.bin' ` : skip .npy files and .bin files (to skip files use `-I` flag(use regular expression after that)).
  
**************************************

- **grep**: 
  `grep =inrI "adabins" ./ --exclude=GLOB --exclude-from=FILE --exclude-dir=GLOB
  **--exclude=GLOB : this works best** (only for files)
  `grep -inrI "Global BA Iteration" ./ --exclude-dir srj_eval_logs --exclude="*eval*"`   (to ignore evaluation_scripts/eval.ipynb and evaluation_scripts/eval2.ipynb)
  `grep -Iinr "adabins" ./ --exclude-dir scripts               --exclude-from=test.py `
  `grep -Iinr "adabins" ./ --exclude-dir={scripts,third_party} --exclude={*.cpp,test1.py,test2.py}`
  ``` bash
  -i ignore case
  -n do numbering for matches
  -r recursive
  -I ignore Binary files
  --include=GLOB : Search only files whose base name matches GLOB
  -R, --dereference-recursive ... Read all files under each directory, recursively.  Follow all symbolic links, unlike -r.
  ```
**************************************
- **sed**: used for <kbd>find and replace</kbd> , <kbd>delete a line</kbd> without opening a file.
  `sed 's/ unix/ linux/' geekfile.txt ` (replace only 1st occurence of `unix` with `linux`)
  `sed 's/ unix/ linux/3' geekfile.txt ` (replace only 3rd occurence of `unix` with `linux`)
  `sed 's/ unix/ linux/g' geekfile.txt ` (replace all occurence of `unix` with `linux`)
  `sed 's/ unix/ linux/3g' geekfile.txt ` (replace only 3rd to all occurence of `unix` with `linux`)
  
  to delete:
  `sed '/pattern/d' filename.txt` (to delete a line having "pattern" string in that).
**************************************
- **Keep log of terminal i/p and o/ps to a file:** When you are ready to start recording a log file, type:

  `script screen.log`  or `script path/where/you/want/to/store/screen.log` 
  Now, until you stop the script, all input and output in the Terminal will be stored in screen.log. When you are done, just type:
  I suggest to also specify your terminal name in .log file:
  `script TUM_window-droid_pane-topright.log` (Note that it doesn't outputs to .log file in realtime, it does after sometime, but if you do `exit`, then it'll update all logs till that time. so have patience it'll update after sometime.)
  `exit`
  - Your screen.log file will stored in the local directory. If you want to redirect it, use an absolute pathname such as `~/screen.log`. This will do exactly what you are looking for.
**************************************
- **gpustat**: `gpustat -p`
- **nvitop**: `nvitop`
**************************************
- **become sudo user || add yourself to the list of sudoers**:
  Edit `/etc/sudoers` by typing `sudo visudo` or `sudo nano /etc/sudoers` then you can find `root ALL=(ALL) ALL` just add a line next to it for your user also `suraj ALL=(ALL) ALL` (suraj is the user that I want to make sudoer)
**************************************
- ### Build any software from source: *usual steps are:*
  **A. If CmakeLists.txt file is present inside repo:**

  1. `git clone xyz_software`  
  2. `cd xyz_software` 
  3. `mkdir build && cd build` 

  **The below 3 commands are used for "creating, building, and installing" an application in Linux.**
    creating: generate Makefile, 
    building: compile to generate binaries, and 
    installing: copying the binaries to desired location to be used directly without specifying the path.

  4. `cmake ..`  #(give the path of "CMakeLists.txt" file which is parent dir i.e: `..`  )
    *above command will generates the necessary build files(`Makefile`) for the application.*

    *If u **DON'T have SUDO ACCESS??** -> above command could also be like this if you want to install in other location:*
    - I ran this command after this step while being inside `build` dir:
    `cmake -DCMAKE_INSTALL_PREFIX:PATH=/home/suraj/opt/ -DEXPORT_BUILD_DIR=ON ../`  
    
    Explanation of above arguments to `cmake`:
    *`DCMAKE_INSTALL_PREFIX:PATH=/home/suraj/opt/`: CMake will generate the necessary build files with the installation directory set to `/home/suraj/opt/`. This means that when you run the `make install` command after building the project, the compiled binaries, libraries, and other files will be installed to the specified directory (`/home/suraj/opt/` in this case `opt` directory will be created in `make install` step) instead of the default system directories such as `/usr/local/` or `/usr/`.*

    *`-DEXPORT_BUILD_DIR=ON`: option is used to export the location of the Ceres Solver build directory to other applications that depend on it. If you don't need this functionality, you can omit.*

  *(below `make` is the most time consuming step, So, increase #threads)*
  5. `make -j4`  *(means use 4 threds while building the application) I did `make -j20` in vision04 server (ran v. quickly)*
    *The above `make` command is used to build the application from the source code using the build files generated by `cmake`. The `-j` option specifies the number of threads to be used for building the application, which can significantly speed up the build process.*

  6. `sudo make install` or `make install` #["sudo" because it will add compiled binaries, libraries, and other files to `/usr/` or `/usr/local/`]
  `sudo make install` will add to `/usr/local/`
  `make install` will add to `/home/suraj/opt/` (create this "opt" directory in ~).
  
  #### Summary for How to build in a remote server where you don't have `sudo` access**:
  ``` bash
  mkdir build; cd build
  cmake -DCMAKE_INSTALL_PREFIX:PATH=/home/suraj/opt/ -DEXPORT_BUILD_DIR=ON ../
  make -j<no_of_free_cpu_available>
  make install
  #then add `build/bin` path to PATH variable:
  - How to append(add something to) PATH : `export PATH="/path/to/your/dir/where/have/your/executable:$PATH"`
    So add this line to `~/.bashrc`:
    `export PATH="/path/to/your/dir/build/bin:$PATH"`
  ```
    

  **B. If Makefile is present inside repo:** So, No need to run cmake here.
  1. `make`
  2. `make install`  #( "make install" just copies compiled files into appropriate locations.)

  **C. When a project does use the Autotools, the normal mantra to install it is:**
  ```
  ./configure
  make
  make install
  ```
  *`./configure --prefix /path/to/new/prefix` will set a variable in the Makefile that is available when compiling the program. `make install` will install it into that location.*

  (above commands may change a bit in some cases, but in general they are like this)
  - I have observed one thing that whatever I am building from source that dir is getting created at `/usr/local/include/` and it's `.so` or `.o` file is getting added in `/usr/local/lib/`.
  - In my case those files inside  `/usr/local/include/` are: `ceres  eigen3  gflags  glog`.
  - Now all the files inside `glog` are header files (.h files).



**************************************
- **wget**: `wget google.com -P <download-location>`
**************************************
- **copy everything in current directory escept one file/dir to a directory**: `cp -r !(dir_abc|dest_dir) ./dest_dir`

**************************************
**$\LaTeX$ in markdown:**
1. inline latex: write inside single `$ $` 
  Example: $V_{sphere} = \frac{4}{3}\pi r^3$

2. display style equation: write inside double `$$ $$`
  Example: $$V_{sphere} = \frac{4}{3}\pi r^3$$
  
**************************************
- **7z**: `7z x data.zip -otarget_dir` (just after -o start target_dir name)
**************************************

**watch** - execute a program periodically, showing output fullscreen

ex: `watch -n 0.5 nvidia-smi` : runs nvidia-smi in every 0.5 seconds. (default is 2sec)
use `-d` to also highlight differences in every iteration. i.e., `watch -n 0.5 nvidia-smi -d`

you can do ls to see how the files are updating. i.e., `watch -n 1 ls`
see `man watch` for more info.

**************************************
- **locate**: 
**************************************
- **`ls`**: 
  *- to see top 5 rows of `ls -l` command:* `ls -l | head -5`<br/>
  *- to ignore some files: `ls --ignore=*.png`*
  **more Flags with ls:**
  `-S`: sort by size. So use `ls -lS` to show big files first.
  `-r`: show the reverse order. So use `ls -lSr` to show small files first.
  `-t`: sort by modification time, newest first
  `-R`: list subdirectories recursively

  less important:
  `-x`: show ls results in row major order(by default it shows in column)(and sorted by name)
  `-d`: list directories themselves, not their content.

*- some more ls aliases (by ashutosh_a in .bashrc)*
  ``` bash
  alias ll='ls -alF'
  alias la='ls -A'
  alias l='ls -CF'
  ```

  ``` bash
    flags: 
    -A almost all
    -C list entries by column
    -F classify
  ```
  **************************************
**sshfs: mount a directory from another server to your server as a dir**:
*all 3 comands are for my device*
1. create the dir: `mkdir TartanAir_mount`
2. mount: `sshfs data@negroni.cse.iitd.ac.in:/home/data/datasets/TartanAir/TartanAir_extracted/ ./TartanAir_mount/`
3. unmount: `fusermount -u ./TartanAir_mount` *after this command "TartanAir_mount" dir will become empty again as it was after step1 and get detached from negroni*
  *Note: even just after mounting, when I tried to unmount, it is giving below error:*
4. If it says `fusermount: failed to unmount ........ Device or resource busy` then just open `htop` and kill one of your sshfs process there itself. (done this several times)
*make sure to unmount before deleting anything, because it will get deleted in the other server as well*

****************************************
preview markdown (.md file) in vscode itself: <kbd> ctrl + shift + v </kbd>

****************************************
- To refresh LibreOffice Calc(Excel of Ubuntu) : Press F3
- I have made a shortcut key manually by pasting some custom code from this link which does refreshing: [link](https://askubuntu.com/questions/1354991/reload-sheet-in-libreoffice-calc#:~:text=You%20should%20now%20be%20able,by%20pressing%20the%20F3%20key)

****************************************
#### paste these files if `git clone` is not working when you are under a proxy:  
Generally when getting this error: `Received HTTP code 302 from proxy after CONNECT`
``` bash
git config --global http.proxy http://xen03.iitd.ac.in:3128  
git config --global https.proxy http://xen03.iitd.ac.in:3128

git config --global http.proxy http://proxy21.iitd.ac.in:3128  
git config --global https.proxy http://proxy21.iitd.ac.in:3128
```

****************************************
#### how to download multiple files/directories from "Index of" websites:

`wget --no-parent -r http://WEBSITE.com/DIRECTORY`
`wget --no-parent -r https://archive.ics.uci.edu/ml/machine-learning-databases/00442/`

****************************************
#### how to see tensorboard logs (from *tfevents.* file):

`tensorboard --logdir <path_to_directory_where_tfevents_files_present>`
`tensorboard --logdir runs/ --port 1111 --samples_per_plugin=images=100`  
- `--samples_per_plugin` is used to mention how many images to show. If mentioned 2, then it will show only 2 images. So if you want to see image at every step, then mention it as total no. of steps(when you are saving the image to writer). If you have total steps as 50500 and I am saving image at every 100 steps, so total images saved = 50500/100 = 505 images... so use `images=505` above.
(typically tf.events* file got saved in runs/ directory inside the project)

- default port is `6006` always.
- *ideally there should be exactly 1 log file in one logdir(that is for one experiment that you have done.).*: otherwise you'll get below mentioned issue:
-  **I'm seeing data overlapped on itself! What gives?** [Source](https://github.com/tensorflow/tensorboard/blob/master/README.md#im-seeing-data-overlapped-on-itself-what-gives)
  *As a workaround, try changing the x-axis display in TensorBoard from `steps`(means global_step that you have given along with scalar,etc) to `wall_time`. This will frequently clear up the issue.*

    If you are seeing data that seems to travel backwards through time and overlap
    with itself, there are a few possible explanations.

    * You may have multiple execution of TensorFlow that all wrote to the same log
    directory. Please have each TensorFlow run write to its own logdir.

      > **Update:** the [experimental `--reload_multifile=true` option][pr-1867] can
      > now be used to poll all "active" files in a directory for new data, defined
      > as any file that received new data within `--reload_multifile_inactive_secs`
      > seconds ago, defaulting to 86400.

    * You may have a bug in your code where the global_step variable (passed
    to `FileWriter.add_summary`) is being maintained incorrectly.
    * It may be that your TensorFlow job crashed, and was restarted from an earlier
    checkpoint. See *How to handle TensorFlow restarts*, below.
- you can open tensorboard in vscode: `ctrl+shift+p` then search for "Launch Tensorboard".  

****************************************
### How to create symbolic links in Linux
`ln -s <source-dir-path> <dest-dir-path>`
ex: `ln -s ~/scratch/Datasets/nyu_depth_v2 ./nyu_depth_v2`

****************************************
ird13289@cse.iitd.ac.in

### jupyter notebook tips

- `Tab` for Autocomplete
- Press `Shift+Tab` to view the documentation for any function
- `%%time` on 1st line of a cell: will calculate the total time required to execute that cell(wall time).
- progress-bar: 
                for i in range(5):
                  for j in range(10):
                    pass

              REPLACED BY: 
                from tqdm import tqdm_notebook
                for i in tqdm_notebook(range(5),desc='1st loop'):
                  for j in tqdm_notebook(range(10),desc='2nd loop'):
                    pass
- Run code from different languages in the notebook: start the cell with:
    %%bash
    %%HTML
    %%python2
    %%python3
- goto `Help>Keyboard Shortcuts` for shortcuts.


**************************************


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



