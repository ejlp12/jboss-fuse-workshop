Online Repository `https://maven.repository.redhat.com/techpreview/all/`

Maven `setting.xml` di direktori `~/.m2/` atau di WIndows ada di `C:\Users\<NAMA_USER>\.m2\`

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <localRepository/>
    <profiles>
        <profile>
            <id>redhat-techpreview-all-repository</id>
            <repositories>
                <repository>
                    <id>redhat-techpreview-all-repository</id>
                    <name>Red Hat Tech Preview repository (all)</name>
                    <url>http://maven.repository.redhat.com/techpreview/all/</url>
                    <layout>default</layout>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                        <updatePolicy>never</updatePolicy>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>redhat-techpreview-all-repository</id>
                    <name>Red Hat Tech Preview repository (all)</name>
                    <url>http://maven.repository.redhat.com/techpreview/all/</url>
                    <layout>default</layout>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                        <updatePolicy>never</updatePolicy>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>redhat-techpreview-all-repository</activeProfile>
    </activeProfiles>
</settings>
```
