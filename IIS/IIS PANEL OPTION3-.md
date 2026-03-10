#failed request traccing rule- rule banao trace karo status code do website me wo aaya toh trace karegs
#handler maping -particular extension kis way me call hoga jaisee koi website php me bani hai uska extension . php hai toh ab konsi library use deal karegi hame wo dekhna hai
 #http redirect - kisi dusre website me redirect karna jab site kam na kar rhi ho ya maintenaance me ho
 1. have option 
 2. tempory kiya toh status code 307 aayega insect me webite ke netwrok me
 3. permanent kiya toh 301
 4. ab redirection hata do toh phir bhi wo redirect webbsite me jayega
 5. ab browser apne cache se hi answer dega
 6. ab hume cache  ya history hatana padega
 7. jab hi wo apni website pe jayega
 8. permanent redirect me har bar server ke pass request jayegi
 #http response heaader - security header add karne ke liye use hota
 website me respone section me dikhega website ke
 -----attacks ke impact ko kam karte
 #ip and domain restriction- kisi particular ip ya netwrok se website ho ya na ho
 #log- konsa visitor kab website ko visit kiya location - inetpub-logs-logfile-
 #mime type- static content ko kaise deliver karna hai
 jaise koi apk file mene place ki website pe but wo show nhi hone dega isliye hum mime me entry kar denge'
 ab mene us website ke data me rakh diya aur directory browssing enable kar di uske bad bhi nhi karne dega
 ab mime jake add karenge .apk aur google se apk ka memi le lenge ab wo use download karrne dega
  #modules me humne jo installation ke smay jo add kiye the feature wo
#request filetring - me konsi chij allow karni hai komsi nhi
ex- jaise apk download na kar pai
#ssl - sirf https pe side ko allow karts
#webdev - konsa user kya resource aceess karsakta hai