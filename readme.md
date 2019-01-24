# Overview

GitHub hosts over 25 million repositories containing applications of all
shapes and sizes. But GitHub is just a start---those applications still
need to get built, released, and managed to reach their full potential.

With the introduction of [Azure DevOps](https://azure.com/devops),
Microsoft offers developers a new continuous integration/continuous
delivery (CI/CD) service called [Azure
Pipelines](https://azure.microsoft.com/services/devops/pipelines/) that
enables you to continuously build, test, and deploy to any platform or
cloud. It has cloud-hosted agents for Linux, macOS, and Windows;
powerful workflows with native container support; and flexible
deployments to Kubernetes, VMs, and serverless environments.

Azure Pipelines provides unlimited CI/CD minutes and 10 parallel jobs to
every GitHub open source project for free. All open source projects run
on the same infrastructure that our paying customers use. That means
you'll have the same fast performance and high quality of service. Many
of the top open source projects are already using Azure Pipelines for
CI/CD, such as Atom, CPython, Pipenv, Tox, Visual Studio Code, and
TypeScript---and the list is growing every day.

In addition to Azure Pipelines, GitHub users can also benefit from
[Azure Boards](https://azure.microsoft.com/services/devops/boards/), a
set of features that enable you to plan, track, and discuss work across
your teams using Kanban boards, backlogs, team dashboards, and custom
reporting. You can link GitHub activities from Azure Boards by
mentioning them in commits and pull requests, and even automate the
state transition of linked work items when pull requests are approved.

In this demo, you'll see how easy it is to set up Azure Pipelines and
Azure Boards with your GitHub projects and how you can start seeing
benefits immediately.

# Key Takeaways

The key takeaways of the demo are:

-   Microsoft provides the only comprehensive DevOps solution that spans
    from development to project management to deployment to operations.

-   It doesn't matter what technologies of processes you're using---even
    setting up a Node.js solution on GitHub to deploy to a Linux
    container that connects to a Cosmos DB is a seamless,
    straightforward experience.

-   Azure offers a practical approach to automation at every step of the
    DevOps lifecycle that enables companies to focus their efforts on
    creating business value.


=============

## Prerequisites


These items are required for this demo.

1.  A GitHub account from <https://github.com>.

2.  An Azure account from <https://azure.com>.

3.  An Azure DevOps account from <https://dev.azure.com>.

4.  ARM Outputs extension installed in your Azure DevOps account from
    <https://marketplace.visualstudio.com/items?itemName=keesschollaart.arm-outputs>.

5.  Git installed from <https://git-scm.com/downloads>.

6.  Visual Studio Code installed from <https://code.visualstudio.com>.

7.  Azure Pipelines extension for Visual Studio Code installed from
    <https://marketplace.visualstudio.com/items?itemName=ms-azure-devops.azure-pipelines>.

8.  GitHub Pull Requests extension for Visual Studio Code installed from
    <https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github>.

## Demo Setup
You will need to perform these steps prior to presenting this demo.


2.  Clone the GitHub repo locally and open it in Visual Studio Code.

3.  Create a new Azure DevOps project, preferably named something like
    "ContosoAir".

4.  Have two separate browser tabs open and logged in: one on the GitHub
    project root and one on the Azure portal.

============

## Demo Scenario

In this demo, we'll be illustrating the integration and automation
benefits of Azure DevOps. We will take on the role of helping a
fictitious airline---Contoso Air---that has developed their flagship web
site using Node.js. To improve their operations, they want to implement
pipelines for continuous integration and continuous delivery so that
they can quickly update their public services and take advantage of the
full benefits of DevOps and the cloud.

The site will be hosted in Azure, and they want to automate the entire
process so that they can spin up all the infrastructure needed to deploy
and host the application without any manual intervention. Once this
process is in place, it will free up their technology teams to focus
more on generating business value.

Task 1 -- Installing Azure Pipelines
------------------------------------

1. Fork the GitHub project at
    [https://github.com/Microsoft/ContosoAir/](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2FMicrosoft%2FContosoAir%2F&data=02%7C01%7Csraj%40microsoft.com%7Cad53721c5cd84d53603808d6436a6fe4%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636770521758495618&sdata=clPq649M5gB1RKos1J6sZZcQrPFUV7CSwUvle6EYlp8%3D&reserved=0). 
1. Switch to the browser tab open to the root of your GitHub fork. It
    should be something like <https://github.com/account/ContosoAir>.

    In this demo, we will help Contoso Air revamp a
    critical component of their DevOps scenario. Like all airlines, they
    rely on their web site to generate and manage business
    opportunities. However, the current processes they have in place to
    move a change from their source code to their production systems is
    time-consuming and open to human error. They use GitHub to manage
    their source code and want to host their production site on Azure,
    so it will be our job to automate everything in the middle. This
    will involve setting up a pipeline so that commits to the GitHub
    repo invoke a continuous integration build in Azure DevOps. Once
    that build is complete, it will invoke a continuous delivery
    deployment to push the bits out to Azure, creating the required
    resources, if necessary. The first thing we need to do is to connect
    GitHub with Azure DevOps, which we can do via the Azure Pipelines
    extension in the GitHub Marketplace.

2.  Navigate to the **GitHub Marketplace**.

    ![](./images/image1.png){width="5.749281496062992in"
    height="0.49993766404199474in"}

3.  Search for **"pipelines"** and click **Azure Pipelines**.  Azure Pipelines is free to use for both public
    and private repos. If you have a need to scale your builds, you can
    add parallel job support for a nominal fee. Installing it into your
    GitHub account involves just a few clicks, and you can configure
    exactly which repos you want to grant it access to.

    ![](./images/image2.png){width="5.634712379702537in"
    height="1.3956583552055992in"}

    

4.  Scroll to the bottom and click **Install it for free**.

    ![](./images/image3.png)

5.  If you have multiple **GitHub** accounts, select the one you forked
    the project to from the **Switch billing account** dropdown.

    ![](./images/image4.png)

6.  Click **Complete order and begin installation**.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML329dc28.PNG](./images/image5.png)

    Note that if you previously installed Azure Pipelines, you may need
    to click **grant this app access** instead.

    ![](./images/image6.png)

7.  Select the repositories you want to include (or **All
    repositories**) and click **Install**.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML349c700b.PNG](./images/image7.png)

Task 2 -- Configuring an Azure Continuous Integration Pipeline
--------------------------------------------------------------
Now that Azure Pipelines has been installed in the GitHub account, we can configure Azure DevOps to use it. We created an
empty Azure DevOps project ahead of time to hold and run the pipelines we need for continuous integration and continuous delivery. The first
thing we'll do is to create the build pipeline.

1.  Select the organization and Azure DevOps project that you want to use. If you do not have one, you can create for free at 

    ![](./images/image8.png)

2.  Select the forked repo.

    ![](./images/image9.png){width="5.145190288713911in"
    height="1.7914424759405074in"}

    \>Every build pipeline is simply a set of tasks. Whether it's copying files, compiling source, or publishing
    artifacts, the existing library of tasks covers the vast majority of scenarios. You can even create your own if you have specialized
    needs not already covered. We're going to use YAML, a markup syntax that lends itself well to describing the build pipeline. Note that
    the Node.js pipeline as a starting point based on an analysis of our source project. We'll replace the contents with the final YAML
    required for our project.

3.  Select the recommended template.

    ![](./images/image10.png)

4.  Replace the default template with the YAML below.

    1.  resources:

        \- repo: self

        queue:

        name: Hosted VS2017

        demands: npm

        steps:

        \- task: CopyFiles\@2

        displayName: \'Copy Files to:
        \$(build.artifactstagingdirectory)/Templates\'

        inputs:

        SourceFolder: deployment

        Contents: \'\*.json\'

        TargetFolder: \'\$(build.artifactstagingdirectory)/Templates\'

        \- task: Npm\@1

        displayName: \'npm custom\'

        inputs:

        command: custom

        verbose: false

        customCommand: \'install \--production\'

        \- task: ArchiveFiles\@2

        displayName: \'Archive \$(Build.SourcesDirectory)\'

        inputs:

        rootFolderOrFile: \'\$(Build.SourcesDirectory)\'

        includeRootFolder: false

        \- task: PublishBuildArtifacts\@1

        displayName: \'Publish Artifact: drop\'

5.  Click **Save and run**.

    ![](./images/image11.png)

6.  Confirm the **Save and run** to commit the YAML definition directly
    to the master branch of the repo.

    ![](./images/image12.png)

7.  Follow the build through to completion.

Task 3 -- Configuring an Azure Continuous Delivery Pipeline
-----------------------------------------------------------

 Now that the build pipeline has been created and the first build has completed, we can turn our attention to creating a
release pipeline. Like the build templates, there are many packaged options available that cover common deployment scenarios, such as
publishing to Azure. But to illustrate how flexible and productive the experience is, we will build this pipeline from an empty template.

1.  Click **Release**.

    ![](./images/image13.png){width="4.572344706911636in"
    height="0.7603215223097113in"}

2.  Click **Empty job**.

    ![](./images/image14.png){width="2.239303368328959in"
    height="0.6457524059492563in"}

    \> The first item to define in a release pipeline is exactly what will be released and when. In our case, it's the output
    generated from the build pipeline. Note that we could also assign a
    schedule, such as if we wanted to release the latest build every
    night.

3.  Click **Add an artifact**.

    ![](./images/image15.png){width="1.7081200787401576in"
    height="2.0309962817147857in"}

4.  Set **Source** to the build pipeline created earlier and **Default
    version** to **Latest**. Change the **Source alias** to
    **"\_ContosoAir-CI"** and click **Add**.

    ![](./images/image16.png){width="2.8538101487314087in"
    height="3.6662084426946633in"}

    \> **Talk track:** As we did with continuous integration starting on
    a source commit, we also want to have this pipeline automatically
    start when the build pipeline completes. It's just as easy.

5.  Click the **Triggers** button on the artifact.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML38b9ea3a.PNG](./images/image17.png){width="1.8125in"
    height="1.2395833333333333in"}

6.  **Enable** continuous integration.

    ![](./images/image18.png){width="3.176686351706037in"
    height="1.2185979877515312in"}

    \> **Talk track:** We also have the option of adding quality gates
    to the release process. For example, we could require that a
    specific user or group approve a release before it continues, or
    that they approve it after it's been deployed. These gates provide
    notifications to the necessary groups, as well as polling support if
    you're automating the gates using something dynamic, such as an
    Azure function, REST API, work item query, and more. We won't add
    any of that here, but we could easily come back and do it later on.

7.  Click the **pre-deployment conditions** button.

    ![](./images/image19.png){width="2.6246719160104988in"
    height="1.072782152230971in"}

8.  Review pre-deployment condition options.

    ![](./images/image20.png){width="3.3537478127734035in"
    height="1.3227515310586178in"}

    \> **Talk track:** In this pipeline, we're going to need to specify
    the same resource group in multiple tasks, so it's a good practice
    to use a pipeline variable. We'll add one here for the new Azure
    resource group we want to provision our resources to. Note that
    there are also a variety of deployment options we can configure, as
    well as a retention policy.

9.  Select the **Variables** tab.

    ![](./images/image21.png){width="4.676498250218723in"
    height="1.3852438757655292in"}

10. **Add** a **resourcegroup** variable that is not currently used by
    an existing resource group in your Azure account (**"contosoair"**
    will be used in this script).

    ![](./images/image22.png){width="3.22876312335958in"
    height="1.7601968503937009in"}

    \> **Talk track:** Also, just like the build pipeline, the release
    pipeline is really just a set of tasks. There are many
    out-of-the-box tasks available, and you can build your own if
    needed. The first task our release requires is to set up the Azure
    deployment environment if it doesn't yet exist. After we add the
    task, I can authorize access to the Azure account I want to deploy
    to and instruct it to use the variable name we just specified for
    the resource group name.

11. Select the **Tasks** tab.

    ![](./images/image23.png){width="4.811898512685914in"
    height="0.9061362642169729in"}

12. Click the **Add task** button.

    ![](./images/image24.png){width="2.0726574803149607in"
    height="0.708244750656168in"}

13. Search for **"resource"** and **Add** an **Azure Resource Group
    Deployment** task.

    ![](./images/image25.png){width="4.957713254593176in"
    height="2.5725951443569555in"}

14. Select the newly created task.

    ![](./images/image26.png){width="4.030745844269466in"
    height="1.3123359580052494in"}

15. Select and authorize an Azure subscription.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML393f8328.PNG](./images/image27.png){width="4.666666666666667in"
    height="0.6666666666666666in"}

16. Set the **Resource group** to **"\$(resourcegroup)"** and select a
    **Location**.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML42d91670.PNG](./images/image28.png){width="3.0in"
    height="1.5in"}

    \> **Talk track:** Rather than having to manually create the Azure
    resources required to host the web app, the team has defined an
    Azure Resource Manager---or ARM---template that describes the
    environment in JSON. This allows the environment definition to be
    updated and managed like any other source file. These were the files
    we copied to the Templates folder during the build pipeline. You can
    also override the template parameters as part of this configuration,
    which we'll do here using my name.

17. Enter the settings below. You can use the browse navigation to
    select them from the most recent build output.

    Template:
    **\$(System.DefaultWorkingDirectory)/\_ContosoAir-CI/drop/Templates/azuredeploy.json**
    Template parameters:
    **\$(System.DefaultWorkingDirectory)/\_ContosoAir-CI/drop/Templates/azuredeploy.parameters.json**

    You will also need to set **Override template parameters** to
    generate an Azure app service name that is globally unique, so your
    name is recommended. For example, if your name is **John Doe**, use
    something like **"-p\_environment johndoe"**. This will be used as
    part of the app service name in Azure, so please limit it to
    supported characters.

    ![](./images/image29.png){width="4.103653762029746in"
    height="2.8017333770778654in"}

    \> **Talk track:** When this task completes, it will have generated
    an Azure resource group with the resources required to run our
    application. However, the ARM template does some processing of the
    variables to generate names for the resources based on the input
    variables, which we will want to use in future tasks. While we could
    potentially hardcode those variables, it could introduce problems if
    changes are made in the future, so we'll use the ARM Outputs task to
    retrieve those values and put them into pipeline variables for us to
    use. This task happens to be a 3^rd^ party task I installed earlier
    from the Visual Studio Marketplace. It contains this and many other
    extensions for Azure DevOps from both Microsoft and 3^rd^ parties.

18. Click the **Add task** button.

    ![](./images/image24.png){width="2.0726574803149607in"
    height="0.708244750656168in"}

19. Search for **"arm"** and select **Learn more \| More information**.
    This will open the GitHub project for this extension in a new tab.

    ![](./images/image30.png){width="5.415989720034996in"
    height="1.645627734033246in"}

20. Click the link to the Visual Studio Marketplace.

    ![](./images/image31.png){width="4.905636482939633in"
    height="1.4998129921259842in"}

21. Close the new tab.

    ![](./images/image32.png){width="3.7182852143482066in"
    height="2.8225634295713036in"}

    \> **Talk track:** Now let's get back to adding the ARM Outputs
    task. The key variable we care about here is the name of the app
    service created, which our ARM template has specified as an output.
    This task will populate it for us to use as the "web" variable in
    the next task.

22. **Add** an **ARM Outputs** task.

    ![](./images/image33.png){width="4.3536220472440945in"
    height="1.8122736220472442in"}

23. Select the newly created task.

    ![](./images/image34.png){width="4.114069335083115in"
    height="1.3539971566054243in"}

24. Select the same subscription from the previous task and enter the
    same resource group variable name.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML42ecfcec.PNG](./images/image35.png){width="3.75in"
    height="1.3645833333333333in"}

    \> **Talk track:** Finally, we can deploy the app service. We'll use
    the same subscription as earlier and specify the web variable as the
    name of the app service we want to deploy to. By this time in the
    pipeline, it will have been filled in for us by the ARM Outputs
    task. Also note that we have the option to specify a slot to deploy
    to, but we'll talk about deployment slots later on.

25. Click the **Add task** button.

    ![](./images/image24.png){width="2.0726574803149607in"
    height="0.708244750656168in"}

26. Search for **"app service"** and **Add** an **Azure App Service
    Deploy** task.

    ![](./images/image36.png){width="4.3536220472440945in"
    height="3.176686351706037in"}

27. Select the newly created task.

    ![](./images/image37.png){width="4.093238188976378in"
    height="2.405949256342957in"}

28. Select the same subscription as earlier.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML38dcffeb.PNG](./images/image38.png){width="4.354166666666667in"
    height="0.7708333333333334in"}

29. Enter the **App Service name** of **"\$(web)"**.

    ![](./images/image39.png){width="2.7392410323709537in"
    height="0.9894597550306212in"}

30. **Save** the pipeline.

    ![](./images/image40.png){width="3.582884951881015in"
    height="0.4686909448818898in"}

Task 4 -- Invoking Continuous Delivery from GitHub to Azure
-----------------------------------------------------------

\> **Talk track:** Now that we have our pipelines in place, it's time to
commit a change to the master branch on GitHub. We're going to pull down
the azure-pipelines.yml file added by Azure DevOps during the build
creation and commit a slight edit to trigger the CI/CD process.

1.  Open the ContosoAir project in Visual Studio Code.

2.  From the **Source Control** tab, select **Sync** from the **More
    Actions** dropdown.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML1364534f.PNG](./images/image41.png){width="4.958333333333333in"
    height="2.5in"}

3.  From the **Explorer** tab, open **azure-pipelines.yml**.

    ![](./images/image42.png){width="2.1768110236220473in"
    height="3.1558552055993in"}

    \> **Talk track:** Before we make our change, let's take a quick
    look at the build tasks. There are four steps required for the
    build. First, deployment templates are copied to a target folder for
    use during the release process. Next, the project is built with NPM.
    After that, the built solution is archived and finally published for
    the release pipeline to access. With the Azure Pipelines extension
    for Visual Studio Code, you get a great YAML editing experience,
    including support for IntelliSense.

4.  Examine the tasks within the pipeline definition.

5.  Make a change to show IntelliSense offered by the extension.

6.  Undo any changes to avoid breaking the definition.

7.  Add a newline to the end of the file. The purpose is simply to
    produce a change to the file you can commit, but without impacting
    the working build.

8.  Press **Ctrl+S** to save the file.

    \> **Talk track:** Now we can commit and push the updated build
    definition to GitHub. This will invoke a continuous integration
    build in Azure DevOps, which will trigger a continuous delivery to
    Azure upon completion.

9.  From the **Source Control** tab, enter a commit message like
    **"Updated build pipeline"** and press **Ctrl+Enter** to commit.
    Confirm if prompted.

    ![](./images/image43.png){width="3.6453772965879265in"
    height="1.6352121609798775in"}

10. Press the **Synchronize Changes** button at the bottom of the window
    to push the commit to the server. Confirm if prompted.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML136c0f0b.PNG](./images/image44.png){width="1.9375in"
    height="0.71875in"}

    \> **Talk track:** Back in Azure DevOps, we can see that our build
    pipeline has kicked off a new build. We can follow as it executes
    the tasks we defined earlier, and even get a real-time view into
    what's going on at each step. When the build completes, we can
    review the logs and any tests that were performed as part of the
    process.

11. Return to Azure DevOps and navigate to the **Builds** hub.

    ![](./images/image45.png){width="1.6039665354330708in"
    height="1.3644127296587927in"}

12. Click the new build.

    ![](./images/image46.png){width="2.520518372703412in"
    height="1.5102274715660542in"}

13. Track the build tasks.

    ![](./images/image47.png){width="6.5in"
    height="3.2263888888888888in"}

14. Follow the build through to completion.

    ![](./images/image48.png){width="5.895096237970254in"
    height="1.7497812773403325in"}

    \> **Talk track:** Now that the build has completed, let's check out
    the release. It was automatically invoked by the successful
    completion of the build pipeline, and we can follow it all the same.
    Since this is the first time we're deploying, Azure will need to
    provision the resources. That can take a minute, so let's check back
    in later.

15. Navigate to the **Releases** hub.

    ![](./images/image49.png){width="1.6039665354330708in"
    height="1.3644127296587927in"}

16. Select the new release. If one is not immediately available, click
    the **Refresh** option.

    ![](./images/image50.png){width="5.332666229221347in"
    height="1.4685662729658793in"}

17. Click **In progress** to follow the release process.

    ![](./images/image51.png){width="2.312211286089239in"
    height="2.041411854768154in"}

18. Note that it will take a few minutes (around 5 at the time of
    drafting) for the app to finish deploying due to heavy first-time
    operations. Move ahead to the next step while it works in the
    backgroud.

    ![](./images/image52.png){width="6.5in"
    height="3.245138888888889in"}

Task 5 -- Reviewing the ARM template
------------------------------------

\> **Talk track:** A lot of what's going on now was directed by the ARM
template the DevOps team put together. Let's take a tour through it to
understand how it's structured.

1.  Return to **Visual Studio Code**.

2.  From the **Explorer** tab, open **/deployment/azuredeploy.json**.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML13deea1f.PNG](./images/image53.png){width="3.1979166666666665in"
    height="2.0625in"}

    \> **Talk track:** The first section defines the parameters the
    template expects. They can all be overridden externally by a
    parameter file, and, in our case, via the pipeline editor. Note that
    they all have default values, except for the environment parameter,
    which must be supplied.

3.  Review **parameters**.

    ![](./images/image54.png){width="3.5620548993875767in"
    height="2.3434569116360455in"}

    \> **Talk track:** The variables section defines a set of template
    variables that can be used to reduce complex expressions into an
    easily manageable term. For example, you can concatenate a
    combination of variables, resource properties, and other accessible
    functions to generate names or parameters for other parts of the
    template. In this case, the app service and Cosmos DB have their
    names generated from the set of variables passed in. Note the
    site\_web\_name variable, which is the name of the app service that
    will be created and the string the release pipeline needs to know
    for deployment.

4.  Review **variables**.

    ![](./images/image55.png){width="5.551388888888889in"
    height="0.8332294400699912in"}

    \> **Talk track:** The resources section defines the actual
    resources to be created in Azure. This section can get pretty
    complex and offers incredible flexibility for defining what your
    application needs, how they are to be configured, and what
    dependencies they have on each other. This template only explicitly
    defines two resources: the Cosmos DB and the app service.

5.  Review **resources**.

    ![](./images/image56.png){width="5.332666229221347in"
    height="1.9372583114610673in"}

    \> **Talk track:** The outputs section provides a way to expose data
    from this template for external services. This is the part that the
    ARM Outputs task will use to populate the web pipeline variable with
    the complex site\_web\_name template variable.

6.  Review **outputs**.

    ![](./images/image57.png){width="5.0618667979002625in"
    height="1.2081824146981628in"}

Task 6 -- Reviewing the Application and Azure Portal
----------------------------------------------------

\> **Talk track:** While the resources finish spinning up, let's take a
quick tour of the Azure portal. It's thoughtfully designed and easy to
use. The navigation on the left hand side provides access to major
platform components, such as app services and virtual machines.
Solutions are organized as resource groups, which are logical
collections of the resources used to run your solution. We'll search for
the solution our release pipeline created earlier.

1.  Switch to the Aure portal tab (<https://portal.azure.com>).

2.  Review the left-hand navigation.

3.  Search for your resource group name and open it.

    ![](./images/image58.png){width="2.239303368328959in"
    height="1.2915048118985126in"}

    \> **Talk track:** Our solution requires three resources in Azure.
    The app service is our web site and the app service plan is the
    virtual server farm the site is deployed to. The Cosmos DB is a
    globally-distributed, multi-model database service instance we're
    using to manage all of the data in the site. Let's take a closer
    look at the app service.

4.  Click the **App Service**. If this isn't available yet, check in on
    the release pipeline to make sure the Azure Deployment task didn't
    fail. Otherwise, refresh occassionaly until it is. Keep in mind that
    the app service will be available before the app itself is deployed,
    so the actual site itself won't be ready until the pipeline
    completes.

    ![](./images/image59.png){width="4.093238188976378in"
    height="1.4269050743657044in"}

    \> **Talk track:** This view is the dashboard for the app service
    and provides convenient access to virtually everything we could ever
    need to do. One of the most useful features for DevOps professionals
    is the "diagnose and solve problems" view that offers quick access
    to a variety of self-diagnostic features. These features include
    checks for typical issues related to availability, performance, and
    configuration.

5.  Click **Diagnose and solve problems**.

    ![](./images/image60.png){width="2.405949256342957in"
    height="1.6664588801399824in"}

6.  Review the options.

    ![](./images/image61.png){width="6.0375in"
    height="3.6486111111111112in"}

    \> **Talk track:** The deployment center provides a single place to
    track deployment, such as those that are automated via pipeline. You
    can also use and track deployment slots, which allow you to have
    additional release targets. For example, you might make it a policy
    to always deploy to the staging slot so that your team has an
    opportunity to review and run tests before making it public.
    Configuring a release pipeline to push to a specific slot is a
    setting available in deployment tasks.

7.  Click **Deployment Center**.

    ![](./images/image62.png){width="2.426780402449694in"
    height="1.6977045056867892in"}

8.  Review the deployment.

    ![](./images/image63.png){width="4.322376421697288in"
    height="1.1665212160979876in"}

    \> **Talk track:** The "application settings" view enables you to
    define system-level settings for the environment, such as versions
    of .NET or PHP. You can also configure virtual applications and
    directories, as well as application-level settings, such as
    connection strings.

9.  Click **Application settings**.

    ![](./images/image64.png){width="2.4784722222222224in"
    height="1.2291666666666667in"}

    \> **Talk track:** Application Insights is one of the most valuable
    services for DevOps teams. It provides performance tracking and
    management features for every level of an application. We haven't
    configured it for this project yet, but once it's in place, you can
    trace an action in a web browser all the way through the web request
    into an API and down to the resources it's dependent on. If there's
    an error somewhere along the way, it's really easy to diagnose the
    cause so that teams spend more time working on improvements than
    troubleshooting.

10. Click **Application Insights**.

    ![](./images/image65.png){width="2.4371948818897637in"
    height="1.9476727909011373in"}

    \> **Talk track:** One of the great benefits of a cloud platform is
    how easily you can scale a platform up and out. For example, we have
    the option here to scale our current application up to use a pretty
    powerful set of virtual hardware. Or, depending on our needs, we can
    use the scale out option to add more instances as well. There is
    even an autoscale option to automatically add and remove instances
    based on load.

11. Click **Scale up**.

    ![](./images/image66.png){width="2.510102799650044in"
    height="1.3644127296587927in"}

12. Review options.

    ![](./images/image67.png){width="8.13263888888889in"
    height="6.5in"}

    \> **Talk track:** In addition to web sites, you can also build and
    deploy web jobs. These are standalone apps or scripts that can be
    invoked via web hook. You can also use them in combination with a
    scheduler to perform regular tasks, such as batch updates to your
    data store.

13. Click **WebJobs**.

    ![](./images/image68.png){width="2.520518372703412in"
    height="2.760071084864392in"}

    \> **Talk track:** There's also a console option for you to explore
    what's going on in your service. For example, let's get a directory
    listing of the web root.

14. Click **Console**.

    ![](./images/image69.png){width="2.3226268591426074in"
    height="3.0308716097987753in"}

15. Execute a **"dir"** command.

    ![](./images/image70.png){width="4.1765616797900265in"
    height="4.051576990376203in"}

    \> **Talk track:** There are also plenty of built in monitoring and
    alerting features. These save you a lot of time so you can focus on
    developing business value instead. And if you're looking for advice
    on places to improve the application, there's the App Service
    Advisor. Things are looking good now, but we'll want to keep an eye
    on this for future suggestions.

16. Review monitoring options.

    ![](./images/image71.png){width="1.9476727909011373in"
    height="1.7601968503937009in"}

17. Click **App Service Advisor**.

    ![](./images/image72.png){width="1.926842738407699in"
    height="1.3956583552055992in"}

18. Review insights.

    ![](./images/image73.png){width="5.655543525809274in"
    height="2.0205807086614174in"}

    \> **Talk track:** That was a quick tour of the app service
    configuration, so let's check out the actual site in the cloud. We
    just took a project in GitHub and set up a sophisticated, automated
    deployment to Azure in mere minutes!

19. Click the **URL** to open the site.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML39096e7d.PNG](./images/image74.png){width="3.0833333333333335in"
    height="0.5208333333333334in"}

20. Review the site. Keep the browser window open for later.

    ![](./images/image75.png){width="4.551514654418198in"
    height="2.447610454943132in"}

Task 7 -- Managing GitHub Projects with Azure DevOps
----------------------------------------------------

\> **Talk track:** Azure DevOps provides a wealth of project management
functionality that spans Kanban boards, backlogs, team dashboards, and
custom reporting. By connecting Azure Boards with GitHub repositories,
you can create links between GitHub commits and pull requests to work
items tracked in Azure Boards. This enables a seamless way for you to
use GitHub for software development while using Azure Boards to plan and
track your work.

1.  Return to the Azure DevOps tab.

2.  Navigate to **Boards \| Backlogs**.

    ![](./images/image76.png){width="1.489397419072616in"
    height="1.885181539807524in"}

    \> **Talk track:** In our scenario, users will need to be able to
    book flights by selecting the cities involved. We will create a new
    user story to sort the airports listed in the booking form in
    alphabetical order by city. Ordinarily we would create the user
    story at a higher level and add tasks to define how the story is to
    be implemented, but for our demo purposes here we'll leave it as a
    single work item.

3.  Click **New Work Item** and add a user story with the title **"User
    can select airport by city"**. Press **Enter** to create.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML9701286.PNG](./images/image77.png){width="5.364583333333333in"
    height="1.0625in"}

    \> **Talk track:** In addition to working with work items in a
    backlog, we have a very flexible Kanban board option. With the
    board, we can edit items on a card in line, or even drag cards
    around to change their state and assignment. Let's take ownership of
    the new user story so we can begin work.

4.  Click **View as board**.

    ![](./images/image78.png){width="4.530683508311461in"
    height="0.9269674103237096in"}

5.  Drag the newly created user story to the **Active** column.

    ![](./images/image79.png){width="4.624421478565179in"
    height="1.885181539807524in"}

6.  Dropping the user story onto the **Active** column assigns it to you
    and sets its **State** to **Active**. Make note of the task ID for
    reference later during a future commit and pull request.

    ![](./images/image80.png){width="2.333041338582677in"
    height="1.781026902887139in"}

\> **Talk track:** In order to complete our integration, we'll need to
wire up a connection between this project and the GitHub repo.

7.  Click **Project settings**.

    ![](./images/image81.png){width="2.7809022309711287in"
    height="0.5624300087489064in"}

8.  Under **Boards**, select **GitHub connections**.

    ![](./images/image82.png){width="1.8122736220472442in"
    height="1.5623042432195975in"}

9.  Click **Connect your GitHub account**.

    ![](./images/image83.png){width="4.811898512685914in"
    height="1.8226891951006123in"}

10. Select the project repo and click **Save**.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML2712de8e.PNG](./images/image84.png){width="4.28125in"
    height="1.4375in"}

    \> **Talk track:** Let's take a look at our deployed site to see
    what the current booking experience is like. As you can see, the
    airports appear to be sorted by airport code, which isn't the
    behavior we want our users to see.

11. Return to the web app tab and click **Login**.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTMLe8868fc.PNG](./images/image85.png){width="1.7083333333333333in"
    height="0.3541666666666667in"}

12. Log in with any email and password.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTMLe88013a.PNG](./images/image86.png){width="4.572916666666667in"
    height="2.8958333333333335in"}

13. Click **Book**.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTMLe876e60.PNG](./images/image87.png){width="1.6979166666666667in"
    height="0.3541666666666667in"}

14. Expand the airport dropdown to note that it's not sorted
    alphabetically by city.

    ![](./images/image88.png){width="4.738990594925634in"
    height="4.457775590551181in"}

Task 8 -- Committing to Complete a Task
---------------------------------------

1.  Return to **Visual Studio Code**.

    \> **Talk track:** We'll start off by creating a new branch for this
    task. The work itself is pretty straightforward. We just need to
    locate the place where airports are provided to the user experience
    and make sure they're being sorted by city name.

2.  Click the **master** branch at the bottom of the window.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML137df3cb.PNG](./images/image89.png){width="1.6145833333333333in"
    height="0.4895833333333333in"}

3.  From the top of the screen, click **Create new branch**.

    ![](./images/image90.png){width="1.9789195100612424in"
    height="0.9477985564304462in"}

4.  Enter the name **"airport-sorting"** and press **Enter**. This will
    activate the new branch.

    ![](./images/image91.png){width="4.738990594925634in"
    height="0.708244750656168in"}

5.  From the **Explorer** tab, open
    **src/services/book.form.service.js**.

    ![](./images/image92.png){width="3.6870395888014in"
    height="3.6245472440944884in"}

6.  Locate the **getForm** function and replace the existing
    **airports** initializer with the code below. This will sort the
    airports by city.

    airports: this.\_airports.getAll().sort(function(first, second) {

    return first.city.localeCompare(second.city);})

    ![](./images/image93.png){width="6.5in"
    height="1.0541666666666667in"}

7.  Press **Ctrl+S** to save the file.

    \> **Talk track:** We'll skip testing this locally for the sake of
    the demo. Instead, we'll commit it using a comment that includes
    special syntax to link it to the Azure Boards task we saw earlier.
    Now this commit will become trackable from project management, as
    long as we include the phrase "Fixes AB\#ID".

8.  Switch to the **Source Control** tab and enter a commit message of
    **"Changes airport sorting. Fixes AB\#3464."**, but replace **3464**
    with the actual ID of the Azure Boards task. Press **Ctrl+Enter**
    and confirm the commit if prompted.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML97d87a7.PNG](./images/image94.png){width="3.875in"
    height="1.28125in"}

9.  Click the **Publish Changes** button at the bottom of the screen.

    ![C:\\Users\\Ed\\AppData\\Local\\Temp\\SNAGHTML13836058.PNG](./images/image95.png){width="2.0104166666666665in"
    height="0.4375in"}

10. When the push has completed, return to the GitHub browser tab.

    \> **Talk track:** With the commit pushed, we'll create a pull
    request to drive those changes back into the master branch. In this
    case we're inheriting the title from the commit, but having the pull
    request mention "Fixes AB\#ID" will link and complete the target
    work item when the pull request is merged.

11. Click **Compare & pull request**, which should appear on its own. If
    not, refresh.

    ![](./images/image96.png){width="4.718160542432196in"
    height="0.7603215223097113in"}

12. Change the **base fork** to point at your project. By default it
    points at the original Microsoft repo, so be sure to change it.

    ![](./images/image97.png){width="6.5in"
    height="1.3694444444444445in"}

13. The title should initialize to the commit message entered earlier.
    Click **Create pull request**.

    ![](./images/image98.png){width="6.5in"
    height="2.1694444444444443in"}

14. Return to Visual Studio Code.

    \> **Talk track:** Now we'll switch to the other side of the pull
    request and take on the role of reviewer. We can use Visual Studio
    Code to check out the pull request, analyze changes, and comment.
    Assuming we trust the fix, we can merge the pull request to update
    master and kick off the CI/CD.

15. Under **GitHub Pull Requests \| All**, right-click the pull request
    and select **Checkout Pull Request**.

    ![](./images/image99.png){width="4.2390529308836395in"
    height="1.4685662729658793in"}

16. Expand the **Changes in Pull Request** tree.

    ![](./images/image100.png){width="2.718409886264217in"
    height="1.458150699912511in"}

17. Select the **Description** from under the original pull request.

    ![](./images/image101.png){width="2.6871642607174104in"
    height="0.9373829833770778in"}

18. Review the details of the pull request.

    ![](./images/image102.png){width="6.5in"
    height="1.5493055555555555in"}

19. Click **Merge pull request** and confirm the merge.

    ![](./images/image103.png){width="3.218347550306212in"
    height="0.5311832895888015in"}

    \> **Talk track:** Once the deployment works its way through build
    and release, we can confirm the new functionality.

20. Follow the CI/CD pipeline through to completion.

21. Refresh the web app site. Return to the booking page (you'll need to
    log in again) and confirm the airports are sorted by city now
    (scroll down past the airports with no city name).

    ![](./images/image104.png){width="5.0618667979002625in"
    height="4.6556681977252845in"}

22. Return to the Azure DevOps tab open to the Kanban board.

    \> **Talk track:** Since the user story we were working on was
    linked in a pull request that was approved, Azure DevOps will
    automatically transition the state of the work item to "Closed". You
    can also see that the related GitHub commits and pull request were
    linked to the work item.

23. The user story should have already moved to the **Closed** state and
    column. Click to open it.

    ![](./images/image105.png){width="2.312211286089239in"
    height="1.8331036745406823in"}

24. The commit and pull request should now be visible under
    **Development**.

    ![](./images/image106.png){width="2.4996872265966754in"
    height="1.5727198162729659in"}

Summary
=======

Many organizations have their projects hosted in GitHub, and we just
showed how you can set up automated deployment to Azure in minutes. And
it doesn't matter what kind of application they're building or what kind
of environment they're deploying to. Once this automation is in place,
companies can turn their focus to developing business value instead of
infrastructure.
