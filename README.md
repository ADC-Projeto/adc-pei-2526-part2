# ADC-PEI 25/26 — First Web Application (Part 2)

Building on Part 1, this version introduces **error handling** with custom error pages. The app now gracefully redirects users to dedicated HTML error pages instead of returning raw server errors.

---

## What changed from Part 1

- The `/rest/utils/hello` endpoint now simulates an error and **redirects to a custom 500 error page**
- Two custom error pages were added under `webapp/error/`:
  - `404.html` — shown when a resource is not found
  - `500.html` — shown when an internal server error occurs
- Static assets were added under `webapp/img/` (images used by the app)

---

## What this app does

The app serves a simple web page with two available services:

- **`/rest/utils/hello`** — intentionally throws an exception and redirects to `/error/500.html`
- **`/rest/utils/time`** — returns the current server time in JSON

---

## Prerequisites

Before you begin, make sure you have the following installed:

- **Google Cloud Account**
  > ⚠️ See the instructions to redeem your cupon in shared materials: **`ADC_Project_Google_Cloud_Account_Creation.pdf`**
- [Java 21](https://www.oracle.com/java/technologies/javase/jdk21-archive-downloads.html)
- [Python 3.10](https://www.python.org/downloads/release/python-3100/) (for the Google Cloud SDK download)
- [Apache Maven](https://maven.apache.org/install.html)
- [Git](https://git-scm.com/)
- [Google Cloud SDK](https://cloud.google.com/sdk/docs/install) (for cloud deployment)
- [Eclipse IDE](https://www.eclipse.org/downloads/) with the Maven plugin

---

## Getting Started

### 1. Fork and clone the repository

Fork the project on GitHub, then clone your fork locally:

```bash
git clone git@github.com:ADC-Projeto/adc-pei-2526-part2.git
cd adc-pei-2526-part2
```

### 2. Import into Eclipse

1. Open Eclipse and go to **File → Import → Maven → Existing Maven Projects**
2. Navigate to the folder where you cloned the project
3. Select it and click **Finish**
4. Eclipse will resolve dependencies automatically — check for any errors in the **Problems** tab

---

## Building the project

From the project root, run:

```bash
mvn clean package
```

If the build succeeds, you'll find the compiled `.war` file at:

```
target/Firstwebapp-0.0.1.war
```

---

## Running locally

Start the local App Engine dev server with:

```bash
mvn appengine:run
```

Then open your browser and go to:

```
http://localhost:8080/
```

---

## Deploying to Google App Engine

### 1. Create a project on Google Cloud Console
Go to https://console.cloud.google.com/ and create a new project. Take note of the Project ID (e.g. `my-apdc-app`).

### 2. Authenticate with Google Cloud

```bash
gcloud auth login
gcloud config set project <your-proj-id>
```

### 3. Deploy

```bash
mvn appengine:deploy -Dapp.deploy.projectId=<your-proj-id> -Dapp.deploy.version=<version-number>

```

After a successful deploy, your app will be live at:

```
https://<your-project-id>.appspot.com/
```

Test the endpoints directly:

```
https://<your-project-id>.appspot.com/rest/utils/hello   ← triggers a 500 redirect
https://<your-project-id>.appspot.com/rest/utils/time    ← returns current server time
```

To test the 404 page, navigate to any non-existent path, for example:

```
https://<your-project-id>.appspot.com/doesnotexist
```

---

## Project Structure

```
src/
└── main/
    ├── java/
    │   └── pt/unl/fct/di/adc/firstwebapp/resources/
    │       └── ComputationResource.java   ← REST endpoints + error handling
    └── webapp/
        ├── index.html                     ← Front page
        ├── error/
        │   ├── 404.html                   ← Custom 404 page
        │   └── 500.html                   ← Custom 500 page
        ├── img/
        │   ├── cat.png
        │   └── jedi.gif
        └── WEB-INF/
            ├── web.xml                    ← Servlet config
            └── appengine-web.xml          ← App Engine config
```

---

## License

See [LICENSE](LICENSE) for details.

---

*FCT NOVA — ADC-PEI 2025/2026*
