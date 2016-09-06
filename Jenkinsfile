
env.GIT_URL="git@github.com:neilhunt1/Dinosaurus-Terraform.git"
stage 'Provision DEV AWS Stack'
node("master"){
	checkout scm
	sh "terraform plan"
}
input "Proceed with plan execution?"
node("master"){
	sh "terraform apply"
	sh "ls -lhr"
	sh('git -c "user.name=Jenkins" -c "user.email=Jenkins@aquilent.com" add terraform.tfstate')
	sh('git -c "user.name=Jenkins" -c "user.email=Jenkins@aquilent.com" commit -m "Jenkins"')
	pushGit()
	//sh('git -c "user.name=Jenkins" -c "user.email=Jenkins@aquilent.com" tag -a '+tagName+' -m "Jenkins"')
}

stage 'Provision PROD AWS Stack'{
	node("master"){
	}
}


def pushGit(){
	withCredentials([[$class: 'FileBinding', credentialsId: 'dinosauruspem', variable: 'PEMKEY']]) {
		sh 'cat $PEMKEY > ~/.ssh/id_rsa'
		sh 'chmod 400 ~/.ssh/id_rsa'
		sh('git -c "user.name=Jenkins" -c "user.email=Jenkins@aquilent.com" push '+env.GIT_URL+' --tags')
	}
}