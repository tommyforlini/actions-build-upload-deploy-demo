# actions-build-upload-deploy-demo

Demo how to build a Go Serverside and React Clientside app, upload to an artifact repo (Github for this demo) and then download an executable (JFrog CLI for this demo) and then run a series of bash commands.

Note: The upload to artifact repo (currently Github for this demo) is to simulate how to push to any type of Artifact Repository `example Artifactory` or `GitHub Packages` (currently only supports Node,Java,Nuget,Docker,Ruby).

Note: The download CLI (currently JFrog CLI for this demo) is to simulate how any CLI example `Provisioning Services CLI` can be downloaded from any source and then execute a series of bash commands like to publish to a PaaS Container.

## Action Details

The file `.github/workflows/main.yml` is the key to have actions running. It uses Ubuntu as a runner.

It imports Go actions, to compile the binary

```yml
    - name: Set up Go 1.13
      uses: actions/setup-go@v1
      with:
        go-version: 1.13
      id: go
```

It imports Node actions, to compile the react app

```yml
    - name: Set up NodeJs 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
        # Could be replaced with any other artifact repo like Artifactory 
        registry-url: 'https://registry.npmjs.org'
      id: node

```

And then uploads the final package to Github

```yml
    - name: Upload artifact
      uses: actions/upload-artifact@v1.0.0
      with:
        # Artifact name
        name: mySimpleGoMuxApp
        # Directory containing files to upload
        path: bin
```

A simulated Deployment then downloads the package prepared from a previous step

```yml
    - name: Download artifact
      uses: actions/download-artifact@v1.0.0
      with:
        # Artifact name
        name: mySimpleGoMuxApp
```

And then runs classic bash scripts

```yml
    - name: Execute CLI
      shell: bash
      run: |
        #Call CLI
        echo "Its time todo this now!!"
        ls -latr
        ./jfrog --version
```