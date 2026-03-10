#input redirect
koi command chala re uska output kisi command ke liye input ho wo standard input kaaheenge
ex=cat /log/var/messages | wc -l
standard input 
input redirect by minima sign [<]
method of input redirect [standard input]
1. redirect frrom file - like wc < user.txt is file me output save ho wha se redirect
ex= wc ek command hai jisme hum user.txt ka output wc me input ki tarah denge
2.  interactive mode  or piping -aur humne command di aur usko usi bhagat ki command me use kar liya ho
ex =id | wc -l or cat ipconfig |wc -l
3. interactive or piping - command ke andar direct switch ho jaise 
ex = wc -l user.txt  
4.  redirect from file -sort < unsorteed .txt
ex - sort no.txt
2.sort -n no.txt
3.sort -n < no.txt
4.cat no .txt 
5. using stdin through binary script 
ex-program se input
ex -program ke andar se input lena hai
6. interactive tools reading from stdin
bc =basic calculator
7. sdin file se redirect by reflleccting like a mirror
 ex= /dev/stdin se
 ex -cat /dev/stdin
  ex -echo "test"
  ex-echo 'test' > /dev/stdin