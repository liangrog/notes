GoLang Notes
============

Go get via ssh
--------------
Up to go 1.8, I am not sure the reason why `go get` doesn't support ssh natively. The workaround is below:

1. Add below to your `~/.gitconfig`

        [url "git@github.com:"]
            insteadOf = https://github.com/

   Or run below:

        $ git config --global url.git@github.com:.insteadof https://github.com/

   This will force git to use ssh. 

2. Run `go get` as below format:

        $ go get github.com/[username]/[reponame]
