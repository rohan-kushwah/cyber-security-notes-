commmand - #usermod same switches as useradd
[-d] kha karna hai home directory create 
[-m] home directory create karna hai
[-M] directory nhi banegi
[-c] for comment " likhna hai comment"  in passwd file
[-N] se user toh baneega par uska group nhi banega
[-s] shell change then shell name from shell file and username
[-u] se uid change kar sakte hai
ab same uid kisi user ko dena ho toh [-o] ka use karenge
[-g] se gib nai nhi de skte hai but new user me purani gid de sakte hai [ change kar sakte hai but pehle sse wo group ki id exist karni chaiye ]
then [-p] for password set  ke liye new  user me
ab wo password set toh ho gaya but uska encrypted way chaiye toh ab uska 
kyuki wo passwd file me without encrypted password dega
encyrpted version dekhenge
openssl passwd 123
toh 123 ka encrypted version de dega
then use place kardo adduser -p "encrypted password" username
then wo passwd file me # adha hi ayega
toh jha bhu doller dikhe uske aage \ laga do
then ab usse user jo banaya  hai wo login hi kar sakta hai
then -[l]for lock the user after locking the account in passwd file there is a ! mark before the password of user
means it is lock
[-U] for unlock the account
[-f] for in inactive status of user
[-G] se secondry group me add kar sakte hai
like usermod -G 'g1,g2,g3' g 
toh wo sabme add hoga
ab aur koi group add karna ho toh [-a] 
root se kisi user ka password chnage kar sakte hai
command [passwd username] 
normal user khud ka password change kar skta hai but required complexity
# user delete-
userdel username
then user delete hoga directory nhi
toh directory hatana hai toh [-r]
user toh delete ho jayega but us user ke group ke member nhi
kai bar user bhi nhi hoga kyunki wo kisi group ka member hai
[-d] se password hata sakte hai 
[-S] check password  has or not
passwd -S username
[-e] for expiry of user 
shadow file me entry karke bhi dekh lo
