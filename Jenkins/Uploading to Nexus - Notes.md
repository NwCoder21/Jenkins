# Uploading to Nexus - Notes


# # Nexus - Types of Maven Repositories

Q1: On Nexus there are three types of maven repositories, `group`, `hosted` and `proxy`. Explain the difference between each of these. 

A1: 
**Hosted Repositories**: A hosted repository is a repository that you manage yourself. This means that you create and manage the artifacts in the repository, and you are responsible for maintaining the repository's content. Hosted repositories are typically used to store artifacts that you have created yourself or that you have obtained from external sources and want to make available within your organization.

**Proxy Repositories**: A proxy repository is a repository that is configured to fetch artifacts from remote repositories when they are requested. This means that when a user requests an artifact from a proxy repository, the proxy repository will first check to see if it already has the artifact in its cache. If the artifact is not in the cache, the proxy repository will request it from the remote repository and store it in its cache for future requests. Proxy repositories are typically used to reduce the amount of time it takes to download artifacts, as well as to improve the reliability of downloads.

**Group Repositories**: A group repository is a logical collection of other repositories, which are typically hosted and/or proxy repositories. Group repositories allow you to treat multiple repositories as if they were a single repository. When you request an artifact from a group repository, Nexus will search each repository in the group until it finds the artifact. This means that you can configure your build tool to use a single repository URL, even though the repository is actually a group of multiple repositories.

In summary, hosted repositories are where you store your own artifacts, proxy repositories are where you cache artifacts from remote repositories, and group repositories are a way to logically group multiple repositories together.

---

# Nexus - Version Policy

Q2: Nexus repository has three types of version policy, `release`, `snapshot` and `mixed`. what do these mean and when to choose which one?  

A2: The version policy in Nexus Repository Manager determines how the repository handles different versions of artifacts. There are three types of version policies: Release, Snapshot, and Mixed. Here's an explanation of each and when to choose which one:

* **Release Policy**: This policy is used for stable versions of artifacts that are ready for production use. Release artifacts have fixed versions, and once published, they are immutable. This means that once a release artifact is published to a release repository, it cannot be modified. Release artifacts are typically used for stable versions of software components that are released to end-users or deployed to production environments.

* **Snapshot Policy**: This policy is used for artifacts that are under development and subject to change. Snapshot artifacts have versions that are suffixed with a timestamp or a unique identifier, indicating that they are works in progress. Snapshot artifacts are mutable, which means that they can be updated or deleted at any time. Snapshot artifacts are typically used for components that are in active development and being deployed to development or staging environments.

* **Mixed Policy**: This policy allows you to have both release and snapshot artifacts in the same repository. This can be useful if you want to store different versions of the same artifact, including both stable and unstable versions, in the same repository. Mixed policy repositories can be useful for certain development workflows where you want to have a single repository to manage both release and snapshot artifacts.

In summary, you should choose the release policy for artifacts that are stable and ready for production use, the snapshot policy for artifacts that are under development and subject to change, and the mixed policy for repositories that need to handle both release and snapshot artifacts.

---

# Nexus - Layout Policy

Q3: Nexus repository has two types of layout policy., `strict` and `permissive`. What does each one mean and when do I know which one to choose?

A3: In Nexus Repository, the layout policy determines how artifacts are stored in the repository. There are two types of layout policy: strict and permissive.

* **Strict layout policy**: This policy enforces a specific structure for artifact storage. In this policy, artifacts are stored in a specific directory structure based on their artifact coordinates (group ID, artifact ID, version, etc.). This policy ensures that artifacts are stored in a predictable and consistent way, which can be useful for some organizations. It also makes it easier to search for and retrieve artifacts based on their coordinates. If you choose this policy, you must follow the exact layout structure as defined by Nexus.

* **Permissive layout policy**: This policy allows artifacts to be stored in any directory structure, without any restrictions. In this policy, the directory structure is not enforced, and artifacts can be stored in any location. This policy is more flexible and can be useful if you have legacy systems or if you want to store artifacts in a specific way that doesn't follow the strict layout policy. However, it may make it more difficult to find and retrieve artifacts.

When choosing a layout policy, you should consider your organization's needs and preferences. If you want a predictable and consistent structure for artifact storage, choose the strict layout policy. If you need more flexibility and control over artifact storage, choose the permissive layout policy.

---

# Nexus - Content Disposition

Q4: Nexus repository has two types of Content Disposition, Inline and Attachment. What does each one mean and when do I know which one to choose?
A4: In Nexus Repository, Content Disposition is a HTTP header that specifies how the server should handle the artifact being downloaded by the client. There are two types of Content Disposition: Inline and Attachment.

•	**Inline**: This Content Disposition tells the client's browser to display the artifact within the browser window. For example, if you download a PDF document with the inline Content Disposition, the PDF will open up in your browser window.

•	**Attachment**: This Content Disposition tells the client's browser to download the artifact as a file attachment, rather than displaying it within the browser window. For example, if you download a PDF document with the attachment Content Disposition, the browser will prompt you to save the file to your local computer.

When choosing between Inline and Attachment, consider the type of artifact you are serving and the user experience you want to provide.
Inline is generally used for artifacts that can be displayed directly in the browser, such as images, HTML pages, or text documents. Using the inline Content Disposition for these artifacts can provide a better user experience because they can be viewed immediately without having to be downloaded.
Attachment is typically used for artifacts that cannot be displayed directly in the browser, such as ZIP files, JAR files, or other binary files. Using the attachment Content Disposition ensures that the file is downloaded to the user's computer, rather than being displayed in the browser, which is usually the desired behaviour for these types of artifacts.
In summary, the choice between Inline and Attachment should be based on the type of artifact being served and the user experience you want to provide.

---

# Nexus - Blob Store 

Q5: In Nexus repository, what is a Blob store?

A5: In Nexus Repository, a Blob store is a storage location where binary artifacts are stored. When you upload an artifact to Nexus, it is stored in a Blob store. The Blob store is a fundamental component of Nexus Repository, and it provides a way to manage and store binary artifacts.

A Blob store is a logical grouping of physical storage locations. It can be backed by various types of storage technologies, such as local file systems, network file systems, cloud storage systems, or databases. Nexus Repository allows you to create multiple Blob stores, each with its own storage location and configuration settings. This provides flexibility and scalability when managing large amounts of artifacts.

One of the key benefits of using a Blob store in Nexus is that it provides a mechanism for deduplication of artifacts. When you upload an artifact to Nexus, the system checks whether the artifact already exists in the Blob store. If it does, the system simply creates a reference to the existing artifact rather than storing a duplicate copy. This helps to reduce storage costs and ensures that artifacts are managed efficiently.

Overall, a Blob store is a core component of Nexus Repository, and it provides a way to manage and store binary artifacts in a flexible and scalable way.

---

# Nexus - Types of Deployment Polices

Q6: In Nexus repository, what are the types of deployment polices and explain each one?   

A6: In Nexus Repository Manager, deployment policies are used to specify the conditions under which artifacts can be uploaded to a repository. There are several types of deployment policies in Nexus, including:

* **Allow Redeploy**: This policy allows an artifact to be uploaded and replaces the existing artifact with the new version. This is the default policy for hosted repositories.
* **Disable Redeploy**: This policy prevents artifacts from being overwritten. Once an artifact has been uploaded to the repository, it cannot be replaced or deleted. This policy is commonly used for release repositories.
* **Read-Only**: This policy prevents any modifications to the repository. Artifacts can be downloaded, but uploads are not allowed.
* **Allow Snapshots**: This policy allows snapshots to be uploaded to the repository. Snapshots are artifacts that are under active development and may be changed frequently. This policy is commonly used for development repositories.
* **Block Snapshots**: This policy prevents snapshots from being uploaded to the repository. This policy is commonly used for release repositories.
* **Strict Content Type Validation**: This policy enforces strict content type validation for uploaded artifacts. Only artifacts with a valid content type can be uploaded.
* **Permissive Content Type Validation**: This policy allows artifacts to be uploaded

---

# mavenAssetList

Q7: Explain the below block of code:

```java
mavenAssetList: [
 [
  classifier: '',
  extension: '',
  filePath: 
 ]
],
```

A7: The above is a declaration of a Maven asset list in the Groovy programming language. 

A Maven asset list is typically used to specify a collection of artifacts that are to be built and managed using the Apache Maven build system. An artifact is a file, such as a JAR, WAR, or ZIP file, that is produced by the build process and that contains the compiled code, resources, and metadata associated with a software project.
The `mavenAssetList` variable in the above code snippet is an array that contains one or more sub-arrays, each of which represents a single artifact. Each sub-array contains three elements, which are described below:

* `classifier`: This is an optional string that provides additional information about the artifact. It is typically used to differentiate between different versions or builds of the same artifact. For example, an artifact with a classifier of "sources" would contain the source code for a project, while an artifact with a classifier of "javadoc" would contain the project's API documentation
* `extension`: This is a string that specifies the file extension of the artifact. For example, a JAR file would have an extension of "jar", while a ZIP file would have an extension of "zip".
* `filePath`: This is a string that specifies the path to the artifact file on the local file system. This path can be absolute or relative to the project directory.

To summarise, each sub-array in the `mavenAssetList` array represents a single Maven artifact and includes the artifact's classifier, file extension, and file path. This information is used by Maven to build and manage the artifacts associated with a software project.

---

# mavenCoordinate

Q8: explain the below and what do I use for each one

```java
mavenCoordinate: [
 artifactId: 
 groupId: 
 packaging: 
 version: 
]
```

A8: The above code block is a declaration of a Maven coordinate in the Groovy programming language.

Maven coordinate is a set of identifiers that uniquely identify a Maven artifact. These identifiers include the artifact's `groupId`, `artifactId`, `version`, and `packaging`. The `mavenCoordinate `variable in the above code snippet is a map that contains these identifiers, as described below:

* `groupId`: This is a string that identifies the group or organization that produces the artifact. It is typically written in reverse domain name notation, such as `com.example`.
* `artifactId`: This is a string that identifies the artifact itself. It is typically a short, descriptive name that indicates the purpose of the artifact, such as `myproject`.
* `version`: This is a string that identifies the version of the artifact. It is typically a series of numbers separated by periods, such as `1.0.0`.
* `packaging`: This is a string that specifies the type of artifact. It is typically one of several predefined values, such as `jar`, `war`, or `pom`.


---














