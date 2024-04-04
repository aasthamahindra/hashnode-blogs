---
title: "Android App Authentication Using Firebase"
datePublished: Fri Dec 02 2022 10:29:49 GMT+0000 (Coordinated Universal Time)
cuid: clb6d6k5f000b08l92wic1yeo
slug: android-app-authentication-using-firebase
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1669801164325/TqwzYje4L.png
tags: firebase, android, android-studio, first-blog, firebaseauth

---

Hello everyone!

Welcome to my first blog! I thought I should start with something that entices the readers as well as myself. So, let us begin with the journey to authenticate an Android application using Firebase's Email/Password and Google authentication. This will give us a roadmap to how we can add Firebase to our app (manually), and then how we can use Firebase to get started with the authentication.

#### Prerequisite:

*   Basic knowledge of Android Studio.
    
*   Good understanding of basic Java and XML.
    

# What is Firebase?

Firebase is a backend-as-a-service. It is a real-time database that allows storing a list of objects in the form of a tree. It is a highly secure, serverless, NoSQL JSON database.

![Features.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669803771277/I4Z6VYQpR.png align="center")

The main services offered by Firebase are:

*   Real-time database
    
*   User authentication
    
*   Hosting
    

In this blog, we will deep dive into the *User Authentication* service for an Android application.

# Getting Started: Creating an Empty Activity

Before we start working on firebase, we need to create a new project in Android Studio using the following steps:

***Step-1:*** Open Android Studio &gt; New Project.

![3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669805128263/8OG7GpUDU.png align="center")

***Step-2:*** Select Empty Activity from the list of activities. Click Next.

![4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669805263664/aSC42PVfs.png align="center")

***Step-3:*** Add the necessary details such as name, save location, language, and minimum SDK (I am using API 21: Android 5.0). Click Finish.

![5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669805285861/LfwwKZzYO.png align="center")

# Integrate Firebase into the App

Now that we have set up our Android application. Let us add Firebase to the application.

***Step-1:*** We have to go to the Firebase console by using the following link [https://firebase.google.com/](Link). Click on Get Started. Now, we first have to create a Firebase project.

![5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669806499883/bl_HrHUok.png align="center")
![6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669806505683/NE0x6qCLd.png align="center")

***Step-2:*** Add Project Name &gt; Continue &gt; Enable Google Analytics &gt; Continue &gt; Choose Google Analytics Account &gt; Create Project.

![Screenshot 2022-11-30 131520.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669806356440/jyqxHAOxs.png align="center")
![Screenshot 2022-11-30 131547.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669806714450/zKBiwAjaa.png align="center")
![Screenshot 2022-11-30 131619.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669806723220/GAOXujocw.png align="center")

***Step-3:*** We have to choose the platform to add Firebase to our application (i.e., Android). Now we have to fill out the following form:

![Blog Cover.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669807289444/Ii8RRLWVw.png align="center")

We have to mention the package name, and the package name is a mandatory and important thing. The package name should be matched with our Android applications package name. We can find the SHA-1 by hitting on the signing report.

***Step-4:*** Click Register App &gt; Download google-services.json. Once downloaded, add the .json file into the project structure as follows:

![Screenshot 2022-11-30 132550.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669807516458/n-nd67BXF.png align="center")

Click Next &gt; (Add Firebase SDK). Go to Android Studio. The build.gradle (Project: ) should look like this:

```java
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath 'com.google.gms:google-services:4.3.13'
    }
}
plugins {
    ...
}
```

The build.gradle (Module: &lt;project-name.app&gt;) should look like this:

```java
plugins {
    id 'com.android.application'
    id 'com.google.gms.google-services'
}
dependencies {
    ...
    // Import the Firebase BoM
    implementation platform('com.google.firebase:firebase-bom:31.1.0')
    // When using the BoM, don't specify versions in Firebase dependencies
    implementation 'com.google.firebase:firebase-analytics'
}
```

Click Next &gt; Continue to console. Now the project is connected to the Firebase Android app.

![Screenshot 2022-11-30 142216.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669807600633/Ufwfl_SPR.png align="center")

# What is Firebase Authentication?

All the server-side components necessary for user authentication are offered by Firebase Authentication. With SDK, Firebase Authentication is simple. Additionally, it offers various user interface libraries that let us access panels while recording it. In addition to passwords and phone numbers, Firebase authentication also supports popular identity providers like Twitter, Facebook, and Google.

### User Lifecycle

An Auth listener gets notified in the following situation:

*   The Auth object finishes initializing, and a user was already signed in from a previous session or has been redirected from an identity provider's sign-in flow.
    

The current user's access token is refreshed:

*   The access token expires.
    
*   The user changes their password.
    
*   The user re-authenticates.
    

# Native Providers: Email / Password

In `AndroidManifest.xml` add:

```java
// Permission request for internet
<uses-permission android:name="android.permission.INTERNET"/>
...
```

In `build.gradle(Module:<project-name.app>)`, add the following dependencies:

```java
dependencies {
    ...
    // Import the Firebase BoM
    implementation platform('com.google.firebase:firebase-bom:31.1.0')
    // When using the BoM, don't specify versions in Firebase dependencies
    implementation 'com.google.firebase:firebase-analytics'
    implementation 'com.google.firebase:firebase-core'
    // Email - Password Login
    implementation 'com.google.firebase:firebase-auth'
}
```

Now, we need to set up or enable authentication methods. For this, we have to enable a Native provider: Email/Password authentication in the Firebase console. Email/Password &gt; Enable &gt; Save.

![Blog Cover (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669810490125/7uMyELyzz.png align="center")

The UI for the application looks like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1669898865870/h3wLYthpv.png align="center")

You can find the XML code here: [https://github.com/aasthamahindra/Firebase-Android/tree/master/app/src/main/res/layout](https://github.com/aasthamahindra/Firebase-Android/tree/master/app/src/main/res/layout)

In `SignUpActivity.java`:

```java
public class SignUpActivity extends AppCompatActivity {

    private EditText emailET, passwordET, confirmPasswordET;
    private AppCompatButton signUpBtn;
    private TextView loginLink;
    private FirebaseAuth mAuth;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sign_up);

        mAuth = FirebaseAuth.getInstance();

        // Method to initialize all the UI components
        initializeUI();
        
        signUpBtn.setOnClickListener(view -> {
            registerNewUser();
        });

        loginLink.setOnClickListener(view -> {
            startActivity(new Intent(SignUpActivity.this, SignInActivity.class));
            finish();
        });
    }

    private void registerNewUser() {

        String email = emailET.getText().toString();
        String password = passwordET.getText().toString();
        String confirmPassword = confirmPasswordET.getText().toString();

        // Error: if email field is empty
        if(TextUtils.isEmpty(email)){
            emailET.setError("Email is required!");
        }
        // Error: if password field is empty
        if(TextUtils.isEmpty(password)){
            passwordET.setError("Password is required!");
        }
        // Error: if confirm password doesn't match password
        if(!password.equals(confirmPassword)){
            confirmPasswordET.setError("Doesn't match with password!");
        }

        // Register user
        mAuth.createUserWithEmailAndPassword(email, password)
                .addOnCompleteListener(task -> {
                    if(task.isSuccessful()){
                        Toast.makeText(SignUpActivity.this, "Registration Successful!", Toast.LENGTH_SHORT).show();

                        startActivity(new Intent(SignUpActivity.this, SignInActivity.class));
                        finish();
                    }  else{
                        Toast.makeText(SignUpActivity.this, "Registration failed! Please try again later", Toast.LENGTH_SHORT).show();
                    }
                });
    }

    private void initializeUI() {
        emailET = findViewById(R.id.email);
        passwordET = findViewById(R.id.password);
        confirmPasswordET = findViewById(R.id.confirmPassword);
        signUpBtn = findViewById(R.id.signup);
        loginLink = findViewById(R.id.loginLink);
    }
```

In `SignInActivity.java`:

```java
public class SignInActivity extends AppCompatActivity {

    private EditText emailET, passwordET;
    private AppCompatButton signInBtn;
    private TextView registerLink;
    private FirebaseAuth mAuth;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sign_in);

        mAuth = FirebaseAuth.getInstance();

        // Initialize UI components
        initializeUI();

        signInBtn.setOnClickListener(view -> {
            loginUser();
        });

        registerLink.setOnClickListener(view -> {
            startActivity(new Intent(SignInActivity.this, SignUpActivity.class));
            finish();
        });
    }

    private void loginUser() {
        String email = emailET.getText().toString();
        String password = passwordET.getText().toString();
        if(TextUtils.isEmpty(email)) {
            emailET.setError("Email is required!");

        }
        if(TextUtils.isEmpty(password)) {
            passwordET.setError("Password is required!");
        }

        mAuth.signInWithEmailAndPassword(email, password)
                .addOnCompleteListener(new OnCompleteListener<AuthResult>() {
                    @Override
                    public void onComplete(@NonNull Task<AuthResult> task) {
                        if(task.isSuccessful()) {
                            Toast.makeText(SignInActivity.this, "Login Successful!", Toast.LENGTH_SHORT).show();

                        startActivity(new Intent(SignInActivity.this, DashboardActivity.class));
                            finish();
                        } else{
                            Toast.makeText(SignInActivity.this, "Login failed! Please try again later", Toast.LENGTH_SHORT).show();
                        }
                    }
                });
    }
    private void initializeUI() {
        emailET = findViewById(R.id.emailSignIn);
        passwordET = findViewById(R.id.passwordSignIn);
        signInBtn = findViewById(R.id.loginBtn);
        registerLink = findViewById(R.id.registrationLink);
    }
}
```

If you want to SignOut as the current user, add a button:

```java
signOut.setOnClickListener(view -> {
            FirebaseAuth.getInstance().signOut();
            Toast.makeText(this, "Signed Out", Toast.LENGTH_SHORT).show();
            startActivity(new Intent(this, MainActivity.class));
            finish();
        });
```

You can see the registered user with some other details in the Firebase console:

![Blog Cover (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669889346984/FgG7LwbM_.png align="center")

# Additional Providers: Google

Another way to add authentication to your application is by using *Additional Providers* such as Google, Facebook, Twitter, Github, etc. Let us understand Google authentication:

We start with enabling Google authentication. Google &gt; Enable &gt; Save.

![Blog Cover.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1669891249158/NCyUAx-BZ.png align="center")

In `build.gradle(Module:<project-name.app>)`, add the following dependencies:

```java
dependencies {
    ...
    // Google sign-in SDK
    implementation 'com.google.android.gms:play-services-auth:21.1.0'
}
```

In `SignInActivity.java`:

```java
public class SignInActivity extends AppCompatActivity {

    private SignInButton googleSignIn;

    //Adding tag for logging and RC_SIGN_IN for an activity result
    private static final String TAG = "GoogleActivity";
    private static final int RC_SIGN_IN = 9001;

    private FirebaseAuth mAuth;
    private GoogleSignInClient mGoogleSignInClient;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
        mAuth = FirebaseAuth.getInstance();

        //Configure Google Sign In
        GoogleSignInOptions gso = new GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestIdToken(getString(R.string.default_web_client_id))
                .requestEmail()
                .build();

        mGoogleSignInClient = GoogleSignIn.getClient(this, gso);
        googleSignIn.setSize(SignInButton.SIZE_WIDE);
        googleSignIn.setOnClickListener(view -> {signInToGoogle();});

        // Checking if the user is signed in (non-null) and update UI accordingly.
        FirebaseUser currentUser = mAuth.getCurrentUser();

        if (currentUser != null) {
            Log.d(TAG, "Currently Signed in: " + currentUser.getEmail());
            Toast.makeText(SignInActivity.this, "Currently Logged in: " + currentUser.getEmail(), Toast.LENGTH_LONG).show();
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        // Result returned from launching the Intent from GoogleSignInApi.getSignInIntent(...);
        if (requestCode == RC_SIGN_IN) {
            Task<GoogleSignInAccount> task = GoogleSignIn.getSignedInAccountFromIntent(data);
            try {
            // Google Sign In was successful, authenticate with Firebase
                GoogleSignInAccount account = task.getResult(ApiException.class);
                Toast.makeText(this, "Google Sign in Succeeded",  Toast.LENGTH_LONG).show();
                firebaseAuthWithGoogle(account);
                startActivity(new Intent(SignInActivity.this, DashboardActivity.class));
                finish();
            } catch (ApiException e) {
                Log.w(TAG, "Google sign in failed", e);
                Toast.makeText(this, "Google Sign in Failed " + e,  Toast.LENGTH_LONG).show();
            }
        }
    }

    private void firebaseAuthWithGoogle(GoogleSignInAccount account) {
        Log.d(TAG, "firebaseAuthWithGoogle:" + account.getId());

        //Calling get credential from GoogleAuthProvider
        AuthCredential credential = GoogleAuthProvider.getCredential(account.getIdToken(), null);
        mAuth.signInWithCredential(credential)
                .addOnCompleteListener(new OnCompleteListener<AuthResult>() {
                    @Override
                    public void onComplete(@NonNull Task<AuthResult> task) {
                        if(task.isSuccessful()) {
                            FirebaseUser user = mAuth.getCurrentUser();
                            Log.d(TAG, "signInWithCredential:success: currentUser: " + user.getEmail());
                            Toast.makeText(SignInActivity.this, "Google Authentication Successful!",  Toast.LENGTH_LONG).show();
                        }else{
                            // If sign-in fails to display a message to the user.
                            Log.w(TAG, "signInWithCredential:failure", task.getException());
                            Toast.makeText(SignInActivity.this, "Authentication failed:" + task.getException(),  Toast.LENGTH_LONG).show();
                        }
                    }
                });
    }

    public void signInToGoogle(){
        //Calling Intent and call startActivityForResult() method
        Intent signInIntent = mGoogleSignInClient.getSignInIntent();
        startActivityForResult(signInIntent, RC_SIGN_IN);
    }

    @Override
    public void onPointerCaptureChanged(boolean hasCapture) {

    }
    ...
}
```

# Conclusion

That’s the simplest solution for handling Firebase authentication with Email/Password as well as Google using a clean architecture. I hope you found this article useful and if you have any questions regarding this topic, feel free and leave a comment in the section below.

Source Code: [https://github.com/aasthamahindra/Firebase-Android](https://github.com/aasthamahindra/Firebase-Android)

If you wanna support me, please [follow me!](https://hashnode.com/@aasthamahindra)