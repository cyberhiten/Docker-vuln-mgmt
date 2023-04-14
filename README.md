## Docker vulnerability management  & SBOM using OSS tools 

I have done Vunerability scans of 30+ Docker images using Trivy & Grype (Anchore CLI earlier) . My current goal is to perform similar scans using various OSS tools like Trivy, Grype & syft for SBOM

##### _This is not a how-to doc , but I will share the bash commands & my history file_

* Trivy scans reports [Trivy reports](https://github.com/cyberhiten/Docker-vuln-mgmt/tree/main/docker-vuln-mgmt-poc/trivy-reports)
* Grype scans reports & _template file html.tmpl_ [Grype reports](https://github.com/cyberhiten/Docker-vuln-mgmt/tree/main/docker-vuln-mgmt-poc/grype-reports)
* Syft reports - software bill of materials (SBOM) [syft reports](https://github.com/cyberhiten/Docker-vuln-mgmt/tree/main/docker-vuln-mgmt-poc/syft-reports)

#### below are the commands I have used to bulk process all images available on local host (Grype, Syft , sfyt with output cycloned-json, trivy default
```
###############grype command default table output
$ for image in $(docker images --format "{{.Repository}}:{{.Tag}}" | grep -v "localhost:5000"); do sudo grype ${image} -o template -t html.tmpl  >  "${image}.html" ; done

##########syft command run for bulk scanning of all images hosted on localhost , default output 
$ for image in $(docker images --format "{{.Repository}}:{{.Tag}}" | grep -v "localhost:5000"); do
    syft packages "$image" -o table > "${image//:/_}.txt"
done
##########sfyt command run for bulk scanning of all images hosted on localhost output cycloned-json 
$ for image in $(docker images --format "{{.Repository}}:{{.Tag}}" | grep -v "localhost:5000"); do
    syft packages "$image" -o cyclonedx-json  > "${image//:/_}.cyclonedx-json"
done
 ############ trivy command 
 $ for image in $(docker images --format "{{.Repository}}:{{.Tag}}" | grep -v "localhost:5000"); do sudo trivy image -f table -o "${image}.txt" $image; done 
```

#### Resolving docker vulnerabilities 
Fixing docker vulnerabilities depends more on re-building and using newer images (considering the fact - docker is immutable by design)
So the legacy VM processes we have for hosts does not work for docker images, the process have to be defined from goundsup, in most cases its easier to go in enforce mode , where older builds are taken offline 
Below are some of the pointers 

* Check if older images can be removed / locked from being used for new projects 
* Ensure newer builds are available for core software (python, java, node, nginx , apache etc)

(Software Bill of Materials) soon. SBOMs are important for vulnerability and patch management of Docker images as they provide transparency into the software supply chain and enable better identification of vulnerabilities and dependencies. With a complete SBOM for a Docker image, it becomes easier to track and manage dependencies, identify potential vulnerabilities, prioritize and patch vulnerabilities, as well as identify alternative components or versions that may be less susceptible to attack. 

SBOMs also help ensure compliance with regulatory requirements and security best practices, such as the NIST Cybersecurity Framework, which emphasizes the importance of supply chain risk management. In the context of Docker images, which can be composed of many different layers and components, a complete SBOM can be especially useful for vulnerability and patch management. By knowing exactly what is included in a Docker image, developers and security professionals can make informed decisions about how to secure and maintain it. This can ultimately help reduce the risk of security incidents and ensure the ongoing security of the software supply chain.




###### [Hiten Desai](https://in.linkedin.com/in/hitendesai) 

<details>
  <summary> collapsible click <-- below is the list of docker images used </summary>

```no-highlight
-------------------------------------------------------------------------------------------



REPOSITORY                    TAG                  IMAGE ID       CREATED         SIZE
httpd                         latest               dc1a95e13784   5 days ago      145MB
registry                      2                    8db46f9d7550   13 days ago     24.2MB
alpine                        latest               9ed4aefc74f6   13 days ago     7.05MB
localhost:5000/alpine         latest               9ed4aefc74f6   13 days ago     7.05MB
nginx                         latest               080ed0ed8312   2 weeks ago     142MB
postgres                      latest               80c558ffdc31   2 weeks ago     379MB
portainer/portainer-ce        latest               a87d51c7a324   6 weeks ago     272MB
localhost:5000/my-portainer   latest               a87d51c7a324   6 weeks ago     272MB
ubuntu                        kinetic-20221130     d6547859cd2f   4 months ago    70.2MB
localhost:5000/ubuntu         kinetic-20221130     d6547859cd2f   4 months ago    70.2MB
redis                         6.2.7                4b1123a829a1   4 months ago    113MB
localhost:5000/redis          6.2.7                4b1123a829a1   4 months ago    113MB
redis                         7.0.3-bullseye       3534610348b5   9 months ago    117MB
localhost:5000/redis          7.0.3-bullseye       3534610348b5   9 months ago    117MB
httpd                         2-alpine3.15         5c2ee73209da   12 months ago   54.9MB
httpd                         2.4-alpine3.15       5c2ee73209da   12 months ago   54.9MB
httpd                         2.4.53-alpine3.15    5c2ee73209da   12 months ago   54.9MB
httpd                         alpine3.15           5c2ee73209da   12 months ago   54.9MB
localhost:5000/httpd          2-alpine3.15         5c2ee73209da   12 months ago   54.9MB
localhost:5000/httpd          2.4-alpine3.15       5c2ee73209da   12 months ago   54.9MB
localhost:5000/httpd          2.4.53-alpine3.15    5c2ee73209da   12 months ago   54.9MB
localhost:5000/httpd          alpine3.15           5c2ee73209da   12 months ago   54.9MB
alpine                        3.13.7               6b7b3256dabe   17 months ago   5.62MB
localhost:5000/alpine         3.13.7               6b7b3256dabe   17 months ago   5.62MB
hello-world                   latest               feb5d9fea6a5   18 months ago   13.3kB
localhost:5000/ubuntu         16.04                b6f507652425   19 months ago   135MB
ubuntu                        16.04                b6f507652425   19 months ago   135MB
ubuntu                        14.04                13b66b487594   2 years ago     197MB
localhost:5000/ubuntu         14.04                13b66b487594   2 years ago     197MB
localhost:5000/ubuntu         bionic-20200311      4e5021d210f6   3 years ago     64.2MB
ubuntu                        bionic-20200311      4e5021d210f6   3 years ago     64.2MB
localhost:5000/alpine         3.10.2               961769676411   3 years ago     5.58MB
alpine                        3.10.2               961769676411   3 years ago     5.58MB
alpine                        3.10.0               4d90542f0623   3 years ago     5.58MB
localhost:5000/alpine         3.10.0               4d90542f0623   3 years ago     5.58MB
amazonlinux                   2.0.20190508         b94321659aca   3 years ago     162MB
localhost:5000/amazonlinux    2.0.20190508         b94321659aca   3 years ago     162MB
centos                        7.6.1810             f1cb7c7d58b7   4 years ago     202MB
centos                        centos7.6.1810       f1cb7c7d58b7   4 years ago     202MB
localhost:5000/centos         7.6.1810             f1cb7c7d58b7   4 years ago     202MB
localhost:5000/centos         centos7.6.1810       f1cb7c7d58b7   4 years ago     202MB
alpine                        3.3                  a6fc1dbfa81a   4 years ago     4.82MB
localhost:5000/alpine         3.3                  a6fc1dbfa81a   4 years ago     4.82MB
python                        3.7.0a3-alpine3.7    dea09749dc41   5 years ago     85.2MB
localhost:5000/python         3.7.0a3-alpine3.7    dea09749dc41   5 years ago     85.2MB
localhost:5000/amazonlinux    2017.03.1.20170812   28b6d09fbbe4   5 years ago     162MB
amazonlinux                   2017.03.1.20170812   28b6d09fbbe4   5 years ago     162MB
debian                        wheezy-20170606      ce2079b0835c   5 years ago     85.1MB
localhost:5000/debian         wheezy-20170606      ce2079b0835c   5 years ago     85.1MB
node                          6.9.2-slim           7805939fcde4   6 years ago     211MB
localhost:5000/node           6.9.2-slim           7805939fcde4   6 years ago     211MB
nginx                         1.8.1-alpine         c0dddb65129b   7 years ago     15.5MB
localhost:5000/nginx          1.8.1-alpine         c0dddb65129b   7 years ago     15.5MB
debian                        6.0                  a873733ef581   7 years ago     76.5MB
localhost:5000/debian         6.0                  a873733ef581   7 years ago     76.5MB
alpine                        2.6                  e738dfbe7a10   7 years ago     4.5MB
localhost:5000/alpine         2.6                  e738dfbe7a10   7 years ago     4.5MB
localhost:5000/redis          3.0.6-alpine         56ddc4d21480   7 years ago     15.9MB
redis                         3.0.6-alpine         56ddc4d21480   7 years ago     15.9MB
nginx                         1.9.5                5e31a05b5e9a   7 years ago     133MB
localhost:5000/nginx          1.9.5                5e31a05b5e9a   7 years ago     133MB
debian                        8.0                  d82f599287cd   7 years ago     125MB
localhost:5000/debian         8.0                  d82f599287cd   7 years ago     125MB
node                          0.12.1-slim          fc2863e27a0a   8 years ago     160MB
localhost:5000/node           0.12.1-slim          fc2863e27a0a   8 years ago     160MB
ubuntu                        10.04                e21dbcc7c9de   8 years ago     183MB
localhost:5000/ubuntu         10.04                e21dbcc7c9de   8 years ago     183MB
----------------------------------------------------------------------------------------------
```

</details>
