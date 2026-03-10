sed command advanced-
  [a-append]  switch use for adding text aage ya niche of text as a line
  ex =[sed '/pattern/a then jo add karna ho' input file]  
  [d] you know for delete the line
  [i-prepend]  switch use for adding text piche ya upar of pattern
  ex =[sed '/pattern/i then jo add karna ho' input file]    
[p] you known for [print]
[q] exit code pattern match me hata dega 
sed '6q' file.txt [stop processing after 5th line]
exit status check karne ke liye echo $?
[s] replacememt 
sed 'S/pattern/PATTERN/' file.txt
[c] change the word 
ex- sed '/root/c  rohan' file.txt
[g] sabpe changes apply karne ke liyer
[i] also use for decoration aur insensitive [-i-------]
[e] command chalane ke liye
ex-[sed '1e date' user.txt]
[e] se do chije ek sath shund sakte hai
ex - sed -ne '/root/p' -e '/armour/p' file.txt
then use particular examples according to their use'
#end

