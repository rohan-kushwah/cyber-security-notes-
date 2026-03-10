yum langavailable se languages pata chalegi
yum distro-sync = distributions update kar dega or update + upgrade ka replace hai 
jab  tak download kar ra tab tak rok sakte hai 
but jab installation stage pe gyaa toh wha nhi rok sakte 
linux uda dega wo kabhi roka toh
ye karne ke bad sare packages update ho jaenge
# yum group-
#um groups" refers to ==a feature within the "yum" package manager (Yellowdog Updater Modified) on Linux systems, where a collection of related software packages are grouped together, allowing users to install or manage a set of packages as a single unit instead of individually installing each package== 
# commands-
yum groups - kitne group hai
yum group list - kon konse hai'
yum group install " then group name" - particular group install karna ho toh
jaise ab minimal ko gui banana hai toh genome desktop enviroment install kar lo
uske bad
#systemctl get-default
#systemctl set-default graphical target 
#systemctl isloate  graphical.target
yum group info group name= info dega'
yum group remove " group name"= remove kr dega
system tool aur development tools install'
agli  bar waps gui chaiye toh start + x command
vim .zsh.bash install kar lo'
# in the case of exe files run in linux-
we use wine
wine - windows ka enviroment bana deta hai phir hum exe ko run kar pai
wo exe run kar sakte ho jinpe dependencies na ho
yum install wine
then yum install wine --skip-broken
nhi ho toh by dnf try kar lo
dnf  config-manager --set -enabled crb = conflict manager on kiya iise
dnf install wine 
ab gui me dikhega 
yum install firefox
gui me jake firefox me tab bhi koi package install na ho toh wo wine se nhi hoga


