```bash
function md {
        mkdir $1
        cd $1
}

function scpp () {
    local USERNAME='dengqiuyang'
    local SERVER='172.20.70.157'
    local OPTIONS='-P 22'
    local DEFAULT_PATH='~/SFTP/'
    local USAGE="usage: scpp [push|pull] [scp options] source target"

    if [ $1 != 'push' ] && [ $1 != 'pull' ] || [ $# -lt 3 ]; then echo $USAGE; return 2; fi
    [ $1 = 'push' ] && scp ${@:2:$#-3} $OPTIONS ${@:(-2):1} $USERNAME@$SERVER:$DEFAULT_PATH${!#}
    [ $1 = 'pull' ] && scp ${@:2:$#-3} $OPTIONS $USERNAME@$SERVER:$DEFAULT_PATH${@:(-2):1} ${!#}
}

```

