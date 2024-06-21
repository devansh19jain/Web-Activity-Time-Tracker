<template>
  <div class="main">
    <template v-if="step === WelcomeStep.InitialView">
      <div class="initial-block">
        <p class="header">{{ t('welcome.message') }}</p>
        <p class="description" v-html="t('welcome.description')"></p>
        <!-- Pills navs -->
        <ul class="nav nav-pills nav-justified mb-3" id="ex1" role="tablist">
          <li class="nav-item" role="presentation">
            <a class="nav-link" :class="{ active: activeTab === 'login' }" @click="switchTab('login')" id="tab-login" href="#pills-login" role="tab"
              aria-controls="pills-login" :aria-selected="activeTab === 'login'">Login</a>
          </li>
          <li class="nav-item" role="presentation">
            <a class="nav-link" :class="{ active: activeTab === 'register' }" @click="switchTab('register')" id="tab-register" href="#pills-register" role="tab"
              aria-controls="pills-register" :aria-selected="activeTab === 'register'">Register</a>
          </li>
        </ul>
        <!-- Pills navs -->

        <!-- Pills content -->
        <div class="tab-content">
          <div class="tab-pane fade" :class="{ show: activeTab === 'login', active: activeTab === 'login' }" id="pills-login" role="tabpanel" aria-labelledby="tab-login">
            <form>
              <!-- Email input -->
              <div data-mdb-input-init class="form-outline mb-4">
                <input type="email" id="loginName" class="form-control" />
                <label class="form-label" for="loginName">Email</label>
              </div>

              <!-- Password input -->
              <div data-mdb-input-init class="form-outline mb-4">
                <input type="password" id="loginPassword" class="form-control" />
                <label class="form-label" for="loginPassword">Password</label>
              </div>

              <!-- Submit button -->
              <button type="submit" data-mdb-button-init data-mdb-ripple-init class="btn btn-primary btn-block mb-4" @click.prevent = "openDashboard">Sign in</button>
            </form>
          </div>
          <div class="tab-pane fade" :class="{ show: activeTab === 'register', active: activeTab === 'register' }" id="pills-register" role="tabpanel" aria-labelledby="tab-register">
            <form>
              <!-- Email input -->
              <div data-mdb-input-init class="form-outline mb-4">
                <input type="email" id="registerEmail" class="form-control" />
                <label class="form-label" for="registerEmail">Email</label>
              </div>

              <!-- Password input -->
              <div data-mdb-input-init class="form-outline mb-4">
                <input type="password" id="registerPassword" class="form-control" />
                <label class="form-label" for="registerPassword">Password</label>
              </div>

              <!-- Repeat Password input -->
              <div data-mdb-input-init class="form-outline mb-4">
                <input type="password" id="registerRepeatPassword" class="form-control" />
                <label class="form-label" for="registerRepeatPassword">Repeat password</label>
              </div>

              <!-- Submit button -->
              <button type="submit" data-mdb-button-init data-mdb-ripple-init class="btn btn-primary btn-block mb-3" @click.prevent="onRegister">Sign in</button>
            </form>
          </div>
        </div>
        <!-- Pills content -->
      </div>
    </template>
    <template v-if="step === WelcomeStep.Tutorial">
      <div class="steps">
        <p class="header">{{ t('getStarted.message') }}</p>
        <p class="description">{{ t('welcomeStart.message') }}</p>
        <p class="step">1. {{ t('pinIcon.message') }}</p>
        <p class="description">
          {{ t('pinIconPart1.message') }}
          <img src="../assets/icons/extension.svg" height="25" /> {{ t('pinIconPart2.message') }}
          <img src="../assets/icons/pin.svg" height="25" />
        </p>
        <img class="img" src="../assets/pin-tutorial.png" height="250" />
        <p class="step">2. {{ t('browse.message') }}</p>
        <p class="description">
          {{ t('browse.description') }}
          <img src="../assets/icons/icon.png" height="35" />
        </p>
        <p class="step">3. {{ t('seeData.message') }}</p>
        <p class="description mt-20">
          {{ t('seeData.description') }}
        </p>
        <div class="btn-block">
          <button class="close" @click="close()">{{ t('close.message') }}</button>
          <form>
            <div class="form-group">
              <label for="exampleInputEmail1">Email address</label>
              <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter email">
              <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
            </div>
            <div class="form-group">
              <label for="exampleInputPassword1">Password</label>
              <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
            </div>
            <div class="form-group form-check">
              <input type="checkbox" class="form-check-input" id="exampleCheck1">
              <label class="form-check-label" for="exampleCheck1">Check me out</label>
            </div>
            <button type="submit" class="btn btn-primary" @click.prevent="openDashboard()">Submit</button>
          </form>
        </div>
      </div>
    </template>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import { useI18n } from 'vue-i18n';
import Browser from 'webextension-polyfill';
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/js/bootstrap.bundle.min.js';

const { t } = useI18n();

enum WelcomeStep {
  InitialView,
  Tutorial,
}

const step = ref<WelcomeStep>(WelcomeStep.InitialView);
const activeTab = ref('login');

function switchTab(tab: string) {
  activeTab.value = tab;
}

async function onRegister() {
  alert('Registered successfully!');
  switchTab('login');
  // const url = Browser.runtime.getURL('src/dashboard.html?tab=dashboard');
  // const tab = await Browser.tabs.query({ currentWindow: true, active: true });
  // Browser.tabs.update(tab[0].id, { url: url });
}

async function onLogin() {
  const url = Browser.runtime.getURL('src/dashboard.html?tab=dashboard');
  const tab = await Browser.tabs.query({ currentWindow: true, active: true });
  Browser.tabs.update(tab[0].id, { url: url });
}

async function openDashboard() {
  const url = Browser.runtime.getURL('src/dashboard.html?tab=dashboard');
  const tab = await Browser.tabs.query({ currentWindow: true, active: true });
  Browser.tabs.update(tab[0].id, { url: url });
}

async function close() {
  const currentTab = await Browser.tabs.getCurrent();
  await Browser.tabs.remove(currentTab.id!);
}

onMounted(() => {
  step.value = WelcomeStep.InitialView;
});
</script>

<style scoped>
.main {
  font-family:Georgia, 'Times New Roman', Times, serif;
  margin: auto;
  text-align: center;
  margin: 0 20%;
  height: 100%;
}
.initial-block {
  margin-top: 20%;
}

.header {
  font-size: 26px;
  font-weight: 700;
}
.img {
  margin: 20px 0;
}
.description {
  font-size: 18px;
}
.description span {
  font-weight: 600;
}
.description img {
  margin: 0 10px;
}
.steps {
  margin-top: 50px;
}
.steps .step {
  text-align: left;
  font-size: 24px;
  font-weight: 700;
}
.steps .step {
  margin: 30px;
}
.steps .description {
  margin: 20px;
}
.next-btn {
  margin-top: 40px;
}
button.close {
  background: #c5c5c5;
  color: rgb(0, 0, 0);
}
</style>
