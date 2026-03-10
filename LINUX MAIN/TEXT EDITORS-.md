koi command likhege aur tab dabainge do bar to uski files aur relatable commands hai wo dikhaiga
ex -cat /et toh hum double tab dabainge toh wo hume puri command likh ke dega [called tab compression]  

vim text editor - latest version of vi
how to command -
1.  jo bhi file ka text edit karna ho vim file name
2. file nhi bhi hogi toh bana dega
#vim basic -
1. vim open karte hi command mode pe le jati hai
2. i or insert karke hum insert mode me ja sakte hai wha : ke sath text likh ke [w] se save [q] se quit aur[!] forefully bhar aayega
3. : last line mode pe ja sakte hai  
4. r se replace mode kisi word ya character pe cursor rakho word replace ho jata hai
5. v se visual mode puri line copy delete wagera kar sakte hai  
6. esc se jis mode pe bhi ho wha se waps command mode par aa jate hai
#insert mode basic -
1. :s/word jo dhundhna ho
2. :%s/word/replace word/g 
3. replace word or word dono me shuru me slash ho toh slash se slash ko kato  
4. ex -%s/\/log/bin /\/etc/var/g
5. :! command jo command doge uska answer mil jayega but wo command file me save nhi hogi q se command mode se bhar aainge 
6. changes hatana ho toh command mode me u for undo
7. :set number = lines ke aage number save ho jayenge  
8. : set nonumber = number hat jayaenge
9. highlight word hatane ke liye [nohl]
#switches of command mode-
hjkl - h for left j for down k for up l for left
1. #character delete- 
1.1 jha curor hai wo word delete karna ho toh [x] wha se teen karna ho toh 3x 
formula = kitte character then x
1.2  capital X jha cursor hai usse piche ke character delete honge
2. #word delete-
2.1 jha cursor hai wha ka word delete karna ho toh [dw]
2.2 bad ke char word hatane ho [4dw] 
3. #line delete-
3.1 wha cursor hai wo line [dd] se delete hogi
3.2 jha cursor hai uske niche ke char line [4dd] 
3.3 jis word me cursor hai uske aage ki particular line delete karni ho toh [D]
3.4 [r] se word replace aur 4 character replace karne ho toh [4r]
3.5 [cw] change word word hata ke insert me le jayega
3.6 [C] capital c utti line hata ke insert me le jayega
3.7 cursor hai uske niche empty line chaiye toh[o]
3.8 cursor ke upar chaiye toh capital [O] 
3.9 join two line jha cursor hai toh [J] lobe  3 karna hai toh[3J] 
3.10 line ko copy karna hai toh [yy] cursor hai wha se char karni hai toh 4[yy]
3.11 paste karna ho toh [p] 
4. #vimtutor se vim sikh sakte hai
5. visual-
5.1 select kari line 
[p] se paste [y] se copy [d] se delete
