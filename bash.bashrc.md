```
# To the extent possible under law, the author(s) have dedicated all 
# copyright and related and neighboring rights to this software to the 
# public domain worldwide. This software is distributed without any warranty. 
# You should have received a copy of the CC0 Public Domain Dedication along 
# with this software. 
# If not, see <https://creativecommons.org/publicdomain/zero/1.0/>. 

# /etc/bash.bashrc: executed by bash(1) for interactive shells.

# System-wide bashrc file

# Check that we haven't already been sourced.
([[ -z ${CYG_SYS_BASHRC} ]] && CYG_SYS_BASHRC="1") || return

# If not running interactively, don't do anything
[[ "$-" != *i* ]] && return

# If started from sshd, make sure profile is sourced
if [[ -n "$SSH_CONNECTION" ]] && [[ "$PATH" != *:/usr/bin* ]]; then
    source /etc/profile
fi

# Warnings
unset _warning_found
for _warning_prefix in '' ${MINGW_PREFIX}; do
    for _warning_file in ${_warning_prefix}/etc/profile.d/*.warning{.once,}; do
        test -f "${_warning_file}" || continue
        _warning="$(command sed 's/^/\t\t/' "${_warning_file}" 2>/dev/null)"
        if test -n "${_warning}"; then
            if test -z "${_warning_found}"; then
                _warning_found='true'
                echo
            fi
            if test -t 1
                then printf "\t\e[1;33mwarning:\e[0m\n${_warning}\n\n"
                else printf "\twarning:\n${_warning}\n\n"
            fi
        fi
        [[ "${_warning_file}" = *.once ]] && rm -f "${_warning_file}"
    done
done
unset _warning_found
unset _warning_prefix
unset _warning_file
unset _warning

# If MSYS2_PS1 is set, use that as default PS1;
# if a PS1 is already set and exported, use that;
# otherwise set a default prompt
# of user@host, MSYSTEM variable, and current_directory
[[ -n "${MSYS2_PS1}" ]] && export PS1="${MSYS2_PS1}"
# if we have the "High Mandatory Level" group, it means we're elevated
#if [[ -n "$(command -v getent)" ]] && id -G | grep -q "$(getent -w group 'S-1-16-12288' | cut -d: -f2)"
#  then _ps1_symbol='\[\e[1m\]#\[\e[0m\]'
#  else _ps1_symbol='\$'
#fi
case "$(declare -p PS1 2>/dev/null)" in
'declare -x '*) ;; # okay
*)
  export PS1='\[\e]0;\w\a\]\n\[\e[32m\]\u@\h \[\e[35m\]$MSYSTEM\[\e[0m\] \[\e[33m\]\w\[\e[0m\]\n'"${_ps1_symbol}"' '
  ;;
esac
unset _ps1_symbol

# Uncomment to use the terminal colours set in DIR_COLORS
# eval "$(dircolors -b /etc/DIR_COLORS)"

# Fixup git-bash in non login env
shopt -q login_shell || . /etc/profile.d/git-prompt.sh

# eval "$(oh-my-posh init bash)"
# oh-my-posh init pwsh --config $env:POSH_THEMES_PATH\montys.omp.json | Invoke-Expression
eval "$(oh-my-posh init bash --config C:/Users/34344/AppData/Local/Programs/oh-my-posh/themes/montys.omp.json)"

export LANG=C.UTF-8
alias ll='ls -lha'
alias la='ls -lah'
alias cdd='cd /d'
alias cl='clear'
alias br='cd ..'
alias tc='cd /d/Code/codedic'

alias cf='tc; cd codeforces'
alias d2='cf; cd Div2'
alias d4='cf; cd Div4'
alias edu='cf; cd edu'

alias atc='tc; cd atcoder'

alias lg='tc; cd luogu/study'

alias nk='tc; cd newcoder/2024牛客寒假训练'

alias main='tc; vi main.cpp'
alias bf='tc; vi bf.cpp'

alias tr='tree -FCH'

cph() { 
    g++ $1.cpp -o $1 -std=c++20 -O2
    echo '----------------------------------------------------------'
    ./$1
    rm $1
    echo '----------------------------------------------------------'
}

v() {
    vim $1.cpp
}

init() {
    cp /d/code/snippets/in .in
    cp /d/code/snippets/ins .ins
}

mk() {
    mkdir $1
    cd $1
    init
}
```
