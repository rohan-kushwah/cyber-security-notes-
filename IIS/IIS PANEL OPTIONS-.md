Here's a concise explanation:


Authentication:


- Verifies the identity of a user, device, or system
- Confirms that the user is who they claim to be
- Typically involves credentials like username/password, biometrics, or tokens
- Grants access to a system, application, or resource

Authorization:


- Determines what actions an authenticated user can perform
- Defines the permissions, rights, and access levels for a user
- Ensures that users can only access resources and perform actions they're allowed to
- Happens after authentication, as it relies on the authenticated user's identity

To illustrate the difference:


1. Authentication: You show your ID to enter a building (proving who you are).
2. Authorization: Once inside, you're only allowed to access certain areas or perform specific actions based on your role or permissions.

In summary:


- Authentication confirms identity
- Authorization defines permissions and access levels
# OPTIONS-
SERVER PE CLICK KARKE LAGAOGE TOH WO SABKE LIYE LAGEGI
ek particular site par lagaoge toh uspe lagegi
aur kisi site ke kisi panel me lagana hai toh bhi lag jayegi
#ab kisi bhi site pe karke dekh lo

#AUTHENTICATION- KON US WEBSITE KO VISIT KAR SAKTA HAI
1. #anonymous matlab sab us website ko visit kar saakte hai
2. #basic authentication- only server users with or without ad users use  give username and password in base64 method for encryption it is easy toh encoded so it is not safe 
3. ex-[Basic aGFjazE6MTIz]
4. #digest authentication- only server users can acees website through it give username and password in  md5 encryption method so it is say to be safe from this password is not given to website so it is secure
5. ex-Digest username=["hack1", realm="Digest", nonce="+Upgraded+v13fd218168705f43cef613048d2963862316b341d4373db0186d1a55f76f723b8fd1bcf392f4687ebf1a3791c62582d427b32969206b86d1c", uri="/", algorithm=MD5-sess, response="55f48a4ecbdf044a685ffd0d1a9d56b7", qop=auth, nc=00000002, cnonce="520d87bf7d338287"]
6. #windows authentication- uses ntlm and kerbos protocol in this kerbos protocol is used it is only use with adds
in kerbos ticket provided so ab id password aur username nhi jayega
ex- 

#AUTHORIZATION- WO USER KYA KYA ACCES KAR SKTA HAI 
1.  anonymous ke sath authorization lagaane ki dikkat nhi baki teen ke sath lagana padega
2. ab add rules me dekh lo
3. all users acces kar sakte hai add ke
4. ab hata diye toh authentication hone ke baujud nhi kar sakta acces
5. ab authentication se hata do aur authorization me anonymous kar do toh bina id password ke visit karne dega
6. ab kisi specific user hi visit kar pai toh wha defined kar do
7. deny role se kisi bhi user ko visit karne se rok sakte hai
8. 
9.   
