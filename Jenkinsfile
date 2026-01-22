pipeline {
	options {
		buildDiscarder(logRotator(numToKeepStr: '10'))
		disableConcurrentBuilds()
		ansiColor('xterm')
	}
	agent any
	stages {
		stage('Build') {
			steps {
				sh 'docker run --rm -v $(pwd):/app -w /app node:lts yarn --registry=https://registry.npmjs.org install'
				sh 'docker run --rm -v $(pwd):/app -w /app node:lts yarn build'
			}
		}
	}
	post {
		success {
			withNPM(npmrcConfig: 'npm-jc21') {
				sh 'docker run --rm -v $(pwd):/app -w /app node:lts npm --registry=https://registry.npmjs.org publish --access public || echo "Skipping publish"'
			}
			withNPM(npmrcConfig: 'npm-github-jc21') {
				sh 'docker run --rm -v $(pwd):/app -w /app node:lts npm --registry=https://npm.pkg.github.com/ publish --access public || echo "Skipping publish"'
			}
			juxtapose event: 'success'
			sh 'figlet "SUCCESS"'
		}
		failure {
			juxtapose event: 'failure'
			sh 'figlet "FAILURE"'
		}
		always {
			sh 'docker run --rm -v $(pwd):/app -w /app node:lts chown -R $(id -u):$(id -g) *'
		}
	}
}
