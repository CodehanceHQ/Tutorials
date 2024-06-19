### Steps to deploy to android

```
# Install Java Development Kit (JDK)
1. Visit the Java SE Development Kit Downloads page: https://www.oracle.com/java/technologies/javase-jdk11-downloads.html
2. Download the appropriate installer for your operating system.
3. Follow the installation instructions for your platform.

# Questions:
# - What is the purpose of installing the Java Development Kit (JDK)?
# - How do you determine which JDK version to download for your operating system?
# - What are the main components included in the JDK?

# Set up Java Environment Variables
# On macOS/Linux:
4. Open your terminal and edit your shell profile (`~/.bashrc`, `~/.zshrc`, or `~/.profile`) to include the following lines:
   export JAVA_HOME=$(/usr/libexec/java_home)
   export PATH=$JAVA_HOME/bin:$PATH
5. Save the file and run:
   source ~/.zshrc  # or ~/.bashrc, or ~/.profile depending on which file you edited

# On Windows:
6. Open the Start Menu, search for "Environment Variables," and select "Edit the system environment variables."
7. Click on "Environment Variables."
8. Under "System variables," click "New" and add:
   Variable name: JAVA_HOME
   Variable value: C:\Program Files\Java\jdk-11.0.x  # Replace with your actual JDK installation path
9. Find the `Path` variable in the "System variables" section, select it, and click "Edit."
10. Click "New" and add:
    %JAVA_HOME%\bin
11. Click "OK" to close all dialog boxes.

# Questions:
# - Why is it important to set the JAVA_HOME environment variable?
# - How do you verify that the JAVA_HOME variable is set correctly?
# - What might happen if the JAVA_HOME variable is not configured properly?

# Verify Java Installation
12. Open a new terminal or command prompt and run:
    java -version
13. You should see the Java version information.

# Questions:
# - What command do you use to verify your Java installation?
# - What information does the `java -version` command provide?
# - How can you troubleshoot if the Java version is not displayed correctly?

# Install Android Studio
14. Download and install Android Studio from the official website: https://developer.android.com/studio
15. Follow the installation instructions specific to your operating system.

# Questions:
# - What are the system requirements for installing Android Studio?
# - What components are installed along with Android Studio?
# - How do you verify that Android Studio has been installed correctly?

# Set up the Android Environment
16. Open Android Studio and follow the setup wizard to install the necessary SDK components.
17. Set the `ANDROID_HOME` environment variable:
   # On macOS/Linux: Add the following lines to your `~/.bashrc` or `~/.zshrc` file:
   export ANDROID_HOME=$HOME/Library/Android/sdk
   export PATH=$PATH:$ANDROID_HOME/emulator
   export PATH=$PATH:$ANDROID_HOME/platform-tools
   export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin

   # Reload your dot file by running this in terminal
   source ~/.zshrc (or whichever shell profile you edited)

   # On Windows: Add the following lines to your System Environment Variables:
   # Open System Properties:
   # Press Win + X and select "System" or right-click on "This PC" and select "Properties".
   # Click on "Advanced system settings" on the left panel.
   # In the System Properties window, click on the "Environment Variables" button.

   # Add ANDROID_HOME:
   # Under System variables, click on "New..."
   # In the New System Variable dialog, enter ANDROID_HOME as the variable name and C:\Users\<YourUsername>\AppData\Local\Android\Sdk (replace <YourUsername> with your actual username) as the variable value.
   # Click OK.

   # Modify PATH:
   # Find the Path variable in the System variables section and select it, then click "Edit..."
   # In the Edit Environment Variable dialog, click "New" and add each of the following paths one by one:
   %ANDROID_HOME%\emulator
   %ANDROID_HOME%\platform-tools
   %ANDROID_HOME%\cmdline-tools\latest\bin
   # Click OK on each dialog box to apply the changes.

   # Verifying the Environment Variables:
   # Open a new Command Prompt and run:
   echo %ANDROID_HOME%
   # You should see the path to your Android SDK.
   # Additionally, check if the paths have been added to the PATH variable by running:
   echo %PATH%
   # You should see the paths you added listed in the output.

# Questions:
# - What is the purpose of setting the ANDROID_HOME environment variable?
# - How do you verify that the ANDROID_HOME variable is set correctly?
# - Why is it important to add Android SDK paths to the PATH environment variable?

# Install Ionic CLI
18. Install the Ionic CLI globally if you haven't already:
   npm install -g @ionic/cli

# Questions:
# - What is the Ionic CLI used for?
# - How do you verify that the Ionic CLI is installed correctly?
# - What are some common commands you can run with the Ionic CLI?

# Log in to Ionic Appflow
19. Log in to your Ionic account:
   ionic login

# Questions:
# - What is the purpose of logging in to Ionic Appflow?
# - How do you resolve issues if the login process fails?
# - What are the benefits of using Ionic Appflow for your projects?

# Initialize Ionic Project
20. Navigate to your Ionic project directory:
   cd /path/to/your/ionic-project

# Add Android Platform
21. Add the Android platform to your project:
   ionic capacitor add android

# Questions:
# - Why do you need to add the Android platform to your Ionic project?
# - What does the `ionic capacitor add android` command do?
# - How do you verify that the Android platform has been added successfully?

# Build the Project
22. Build your project to ensure everything is set up correctly:
    ionic build
    npx cap sync

# Questions:
# - What is the purpose of building your Ionic project?
# - How do you verify that the build process was successful?
# - What does the `npx cap sync` command do?

# Open the Android Project
23. Open the Android project in Android Studio:
   npx cap open android

# Questions:
# - Why do you need to open the Android project in Android Studio?
# - What should you check for when the Android project opens in Android Studio?
# - How do you troubleshoot if the project fails to open correctly?

# Configure Signing Key (Optional for Release Builds)
24. Generate a signing key for your app if you are building a release version:
    # Run the keytool command to generate the keystore:
    keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias sourcing-app-key

    # Follow the prompts to enter the necessary information:

    # Enter keystore password:
    # Choose and enter a secure password.

    # Re-enter new password:
    # Confirm the password by entering it again.

    # Enter your first and last name:
    # Provide your name.

    # Enter your organizational unit:
    # Provide your organizational unit (e.g., "Development").

    # Enter your organization:
    # Provide your organization name (e.g., "MyCompany").

    # Enter your City or Locality:
    # Provide your city or locality.

    # Enter your State or Province:
    # Provide your state or province.

    # Enter your two-letter country code:
    # Provide your country code (e.g., "US" for United States).

    # Is CN=Your Name, OU=Development, O=MyCompany, L=City, ST=State, C=US correct? [no]:
    # Confirm that the information is correct by typing "yes".

    # Enter key password for <sourcing-app-key> (RETURN if same as keystore password):
    # Press Enter if you want to use the same password as the keystore password, or enter a different key password.

    # Once you complete these steps, the keystore file (my-release-key.jks) will be generated and stored in your current directory. Keep this file and the password safe, as they are essential for signing your app.

# Questions:
# - What is the purpose of generating a signing key for your Android app?
# - How do you keep your keystore file and password secure?
# - Why is it important to follow the prompts accurately when generating the keystore?

# Deploy via Appflow
26. Push your code to a GitHub repository.  
27. In the Ionic Appflow dashboard, create a new app (import) and link it to your GitHub.
28. Set up the build configurations in Appflow for Android.
29. Start a new build in Appflow.
30. Once the build is complete, download the APK from Appflow.
31. Click on QR code on build to install on device.

# Questions:
# - How do you push your code to a GitHub repository?
# - What steps are involved in linking your GitHub repository to Ionic Appflow?
# - How do you initiate and monitor a build process in Appflow?

Your Ionic Angular project is now deployed to an Android device via Appflow.
```

