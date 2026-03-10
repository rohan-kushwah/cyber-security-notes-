#pipe and redirect
#ek bar me sari command run kar sakte hai series wise

1.1 use colon; space
1.2 use && space
1.3 space hatane se koi dikkat nhi hogi
1.4 use & single and se bhi run hojayegi command but wo command ko background me put kar degi
for free of mouse
1.5redirect command ko kar denge> /dev/null/ to wo command redirect  hoke bacckground me chali jayegi
redirect hone ke bad jo id ayegi wo processs id hogi
1.6 jo command bacckground me jayegi use hum jobs command deke dekh sakte hai
1.7 wapas commandd ko front pe lane ke liye fg command dena padega reverse pattern me
1.8 pehli command chalegi jab dusri chaleegi

# double & and colon; me pehli command jab tak run hogi useke badd hi dusri hogi but single & me usse wo terminate kar dega or dusri chala dega
  #2 pie|  command 
  2.1 pattern same like redirect but pie give output of last command
  2.2 work in the pattern of standard output iskA OUTPut iske sar
  ex =like cat /var/log/messages | wc -l
  ex = cat /var/log/messages | less
  2.3 standard 1 for output redirects standard 2 for error redirects and standard 0 for inputs redirects
  2.4 koi bhi command ke bunch ko sath me chalinge toh do  case bante hai=
  2.4.1=output
  2.4.2=error
  2.4.3=dono
  ex= cat /etc/shadow /etc/os-release dono  aayenge output or error
  2.5 jo command chalate hai uska jo output aata hai  use bad hum  check karenge exit status yadi wo 0 aya toh use standard output bolte hai
  iska exit status hota hai har  command ke chalne ke bad ek variable aat hai o aaya toh command succesfully done aur kuch aay toh wo command sahi se nhi run hui
  exit status check karne ke liye echo $?
  2.6 output redirect jiska output samne nhi dikhega 
  ex= id > user_id.txt
  toh command ka output is file me dikhega
  2.7 error redirect 
  ex= cat /etc/shadow 1> 1.txt
  one dala isliye output  redirect ho jayega
  ex=cat /etc/shadow 2> 2.txt
  2 dala  isliye error  redirect ho jayega
  2.8 bunch of command di usme ek error and output aa ra toh hum usme dono ko redirect kar sakte hai 
  ex =cat /etc/shadow /etc/os -release > output.txt output redirect\
  ex=cat /etc/shadoew /etc/os -release >output.txt 2> error.txt  dono redirect ho jayenge
  dono chije ek file me toh = & laga denge
 ex =cat /etc/shadow /etc/os -release &> output-error.txt
 dono ko same  jagh ka bhi ek hi fanda ex =cat /etc/shadow /etc/os -release 1> output-error.atxt 2>&1
