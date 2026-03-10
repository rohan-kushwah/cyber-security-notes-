# awk patterns=
awk last 
! used for not
ex - awk '/root/{print $0}'
toh ye root layeega
ex - awk '!/root/{print $0}' 

ab root ke alawa wala layega 
bash aur passwd me bhi first last dekh lo 
bin bash me slash structure dhyan rakhan
ab aisa line aur field or para chaiye jiska pehla word root ho toh 
^ upper case use karenge
ex - awk '/^root/{print $0}' 
ab aisa line aur field or para chaiye jiska last word root ho toh 
ex - awk '/root$/{print $0}' 
ye sab ps -aux me karke dekh lo 
started session me
ifconfig me jitti way me grep ka sakte ho karke dekh lo aur awk ki help se  bhi
# number of field-[NF] 
field dekhegea
ex -echo " one two three four " | awk '{print NF}'
toh jitti fields hai lake dega
ex -echo " one two three four " | awk '{print "NF-1"}'
third lake dega 
aise me kabhi$NF kar ra toh wo last field layega
ex -echo " one two three four " | awk '{print "NF-2"}'
2 lake dega
ex -echo " one two three four " | awk '{print "NF-1"}' 
first lake dega
ex - echo "one two three four " | awk '{print $1, $NF, $(NF-1)}' 
HOW to use in file
ex - awk '{print NF}' /etc/passwd
file seperator de dena
ps -aux me dekh lo
 then ifconfig me bhi karke dekh lo
 # number of records - line number
 [NR] 
 dono ko sath me chala ke dekh lo
 # [FS] - FIELD SEPERATOR
 EX - ECHO "one two three four" | awk 'BEGIN{FS="two"} {print $1}' 
 two ab field 1 one and field 2 two
 record seperator jo bhi do uske age nayya line aa Jayega
 EX - awk 'BEGIN{RS=":"} {print $0}'  /etc/passwd
 THEN LINE CHECK KAR LO
  [NR] - LINES DEGA
  [RS] - CHARACTER AND SPACE COUNT KARKE ANSWER DEGA
  most of the time rs and fs same