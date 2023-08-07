<template>
  <section class="flex h-screen w-full">
    <div
      class="w-1/2"
      style="
        background-image: url(https://images.unsplash.com/photo-1682687220742-aba13b6e50ba?ixlib=rb-4.0.3&ixid=M3wxMjA3fDF8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1470&q=80);
        background-repeat: no-repeat;
        background-size: cover;
      "
    >
      <h1 class="mb-5 text-2xl mt-10 ml-10 text-white">Hi There!</h1>
    </div>
    <div class="w-1/2 flex flex-col justify-center items-center bg-gray-200">
      <template v-if="!loggedIn">
        <h2 class="mb-5 text-xl">Login or register</h2>
        <div class="w-full max-w-xs">
          <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
            <div class="mb-4">
              <label
                class="block text-gray-700 text-sm font-bold mb-2"
                for="username"
              >
                Username
              </label>
              <input
                v-model="loginData.userName"
                class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                id="username"
                type="text"
                placeholder="Username"
              />
            </div>
            <div v-if="!signIn" class="mb-4">
              <label
                class="block text-gray-700 text-sm font-bold mb-2"
                for="username"
              >
                Name
              </label>
              <input
                v-model="loginData.name"
                class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                id="username"
                type="text"
                placeholder="Full name"
              />
            </div>
            <div class="">
              <label
                class="block text-gray-700 text-sm font-bold mb-2"
                for="password"
              >
                Password
              </label>
              <input
                v-model="loginData.password"
                class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
                id="password"
                type="password"
                placeholder="******************"
              />
            </div>
            <div class="mb-6" v-if="!signIn">
              <label
                class="block text-gray-700 text-sm font-bold mb-2"
                for="reenter-password"
              >
                Re-enter Password
              </label>
              <input
                v-model="loginData.reenteredPassword"
                class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
                id="reenter-password"
                type="password"
                placeholder="******************"
              />
              <!-- Show error label if passwords don't match -->
              <p v-if="!isPasswordMatch" class="text-red-500 text-sm">
                Passwords do not match.
              </p>
            </div>
            <div class="flex items-center justify-between">
              <button
                class="text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
                :class="
                  !isPasswordMatch ||
                  loginData.userName == '' ||
                  loginData.password == ''
                    ? 'bg-orange-600 '
                    : 'bg-green-600 hover:bg-green-700'
                "
                type="button"
                @click="signInOrRegister()"
                :disabled="!isPasswordMatch"
              >
                {{ buttonLabel }}
              </button>
              <a
                class="inline-block align-baseline font-bold text-sm text-orange-600 hover:text-orange-800"
                href="#"
                @click="signIn = !signIn"
              >
                {{ registerOrSignInLabel }}
              </a>
            </div>
          </form>
          <p class="text-center text-gray-500 text-xs">
            &copy;2023 All rights reserved.
          </p>
        </div>
      </template>
      <template v-else>
        <h2 class="mb-5 text-xl">Hello, {{ loggedInUserName }}!</h2>
        <button
          class="bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="button"
          @click="loggedIn = !loggedIn"
        >
          Go Back!
        </button>
      </template>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref, computed } from "vue";

const users = reactive([]);

const loginData = reactive({
  userName: "",
  password: "",
  name: "",
  reenteredPassword: "",
});

const signIn = ref(true);

const buttonLabel = computed(() => (signIn.value ? "Sign In" : "Register"));
const registerOrSignInLabel = computed(() =>
  signIn.value ? "Or Register" : "Or Sign In"
);

const isPasswordMatch = computed(() => {
  if (signIn.value) {
    // If signing in, don't need to check password match
    return true;
  } else {
    // If registering, check if both passwords match
    return loginData.password === loginData.reenteredPassword;
  }
});

const loggedIn = ref(false);
const loggedInUserName = ref();

function userSignIn() {
  const user = users.find(
    (user) =>
      user.userName === loginData.userName &&
      user.password === loginData.password
  );

  if (user) {
    console.log("User logged in successfully!", user);
    loggedInUserName.value = user.name;
    loggedIn.value = true;
    // Clear the form fields
    loginData.userName = "";
    loginData.password = "";
    loginData.name = "";
    loginData.reenteredPassword = "";
    // Show an alert for successful login
    alert("Login successful!");
  } else {
    console.log("Invalid credentials");
    // Show an alert for unsuccessful login
    alert("Invalid credentials. Please try again.");
  }
}

function registerUser() {
  // Check if any of the fields are empty
  if (
    loginData.userName === "" ||
    loginData.password === "" ||
    loginData.name === "" ||
    loginData.reenteredPassword === ""
  ) {
    console.log("Please fill in all fields.");
    // Show an alert for missing fields
    alert("Please fill in all fields.");
  } else {
    const existingUser = users.find(
      (user) => user.userName === loginData.userName
    );

    if (existingUser) {
      console.log(
        "Username already exists. Please choose a different username."
      );
      // Show an alert for username already exists
      alert("Username already exists. Please choose a different username.");
    } else {
      users.push({
        id: users.length + 1,
        userName: loginData.userName,
        password: loginData.password,
        name: loginData.name,
      });
      console.log("User registered successfully!");
      // Clear the form fields
      loginData.userName = "";
      loginData.password = "";
      loginData.name = "";
      loginData.reenteredPassword = "";
      // Show an alert for successful registration
      alert(
        "Registration successful! You can now sign in with your new account."
      );
    }
  }
}

function signInOrRegister() {
  if (signIn.value) {
    userSignIn();
  } else {
    registerUser();
  }
}
</script>

<style scoped></style>
