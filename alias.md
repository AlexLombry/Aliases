#Some useful alias for MacOSX and Linux

Get a better life with those simple alias
---------------

alias lr='ls -R | grep ":$" | sed -e '\''s/:$//'\'' -e '\''s/[^-][^\/]*\//--/g'\'' -e '\''s/^/   /'\'' -e '\''s/-/|/'\'''

alias ifconfig-ext='curl ifconfig.me'

alias c='clear'

alias snifff="sudo ngrep -d 'en1' -t '^(GET|POST) ' 'tcp and port 80'"

alias process='ps -ax'

alias erase='shred -n 35 -z -u'

alias dss='du -sck * | sort -n'

extract () {
    if [ -f $1 ] ; then
      case $1 in
        *.tar.bz2)   tar xjf $1     ;;
        *.tar.gz)    tar xzf $1     ;;
        *.bz2)       bunzip2 $1     ;;
        *.rar)       unrar e $1     ;;
        *.gz)        gunzip $1      ;;
        *.tar)       tar xf $1      ;;
        *.tbz2)      tar xjf $1     ;;
        *.tgz)       tar xzf $1     ;;
        *.zip)       unzip $1       ;;
        *.Z)         uncompress $1  ;;
        *.7z)        7z x $1        ;;
        *)     echo "'$1' cannot be extracted via extract()" ;;
         esac
     else
         echo "'$1' is not a valid file"
     fi
}


function mcd() {
  mkdir -p "$1" && cd "$1";
}

b() {
    str=""
    count=0
    while [ "$count" -lt "$1" ];
    do
        str=$str"../"
        let count=count+1
    done
    cd $str
}

ft() {
  find . -name "$2" -exec grep -il "$1" {} \;
}


findreplace(){
    printf "Search: ${1}\n"
    printf "Replace: ${2}\n"
    printf "In: ${3}\n\n"

    find . -name "*${3}" -type f | xargs perl -pi -e 's/${1}/${2}/g'
}

grepit(){
    find . -name "*${2}" -print | xargs grep -nir "${1}"
}

function authme {
  ssh "$1" 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys' \
    < ~/.ssh/id_dsa.pub
}

sniff () { sudo ngrep -d ${1} -t '^(GET|POST) ' 'tcp and port 80'; }

function createTunnel()
{
  if [ $# -eq 3 ]
  then
    user=$1
    host=$2
    localPort=$3
    remotePort=$3
  else
    if [ $# -eq 4 ]
    then
      user=$1
      host=$2
      localPort=$3
      remotePort=$4
    else
      echo -n "User: "; read user
      echo -n "host: "; read host
      echo -n "Distant host: "; read remotePort
      echo -n "Local port: "; read localPort
    fi
  fi
  ssh -N -f $user@$host -L ${localPort}:${host}:${remotePort}
}