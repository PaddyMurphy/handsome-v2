<!--
  - NOTE: using vuejs only for the dev environment
  - coded so it easily be transferred to a static page
-->
<template>
  <div id="app" :class="{ open: menuOpen }">
    <div class="logo"><b class="show-accessible">Southwestern University</b></div>

    <label class="menu-label">
      <button @click="toggleMenu" role="button" aria-label="toggle menu" class="button menu-link">
        <svg width="13px" height="10px" viewBox="0 0 13 10" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <g class="icon-menu-open" fill-rule="evenodd" stroke-linecap="square">
              <path class="icon-menu-1" d="M0,1 L10.9655725,1"></path>
              <path class="icon-menu-2" d="M0,5 L8,5"></path>
              <path class="icon-menu-3" d="M0,9 L10.9655725,9"></path>
            </g>
        </svg>
      </button> <b>Menu</b>
    </label>

    <div class="header"></div>
    <!-- TODO: can I put the nav data in a json file and dynamically create? -->
    <div class="menus-container is-hidden">
      <div class="menus">
        <ul class="menu-primary">
          <li><a href="" @mouseover="animateSecondary">Becoming a Student</a></li>
          <li><a href="">Current Students</a></li>
          <li><a href="">Parents & Family</a></li>
          <li><a href="">Faculty & Staff</a></li>
          <li><a href="">Alumni</a></li>
        </ul>

        <ul class="menu-secondary">
          <li><a href="">About NOLS</a></li>
          <li><a href="">Majors & Minors</a></li>
          <li><a href="">Visit Campus</a></li>
          <li><a href="">Financial Aid</a></li>
          <li><a href="">Apply</a></li>
          <li><a href="">Athletics</a></li>
          <li><a href="">Study Abroad</a></li>
          <li><a href="">Alumni Success Stories</a></li>
          <li><a href="">Registrar & Records</a></li>
          <li><a href="">Student life</a></li>
        </ul>
      </div>

      <div class="side-content-wrapper">
        <div class="side-content">

          <div class="card">
            <img src="./img/side-forbes.png" alt="">
            <small class="card-subtitle">ABOUT NOLS</small>
            <h2 class="title">FORBES</h2>
            <p class="info">Forbes names NOLS as the #1 outdoor education school in its 2016 Grateful Grad Colleges. NOLS is also recognized as one of Forbes’ top 50 colleges in the South.</p>
          </div>

          <div class="card">
            <img src="./img/side-jacobswell.png" alt="">
            <small class="card-subtitle">ATHLETICS</small>
            <h2 class="title">Jacob's Well Diving</h2>
            <p class="info">NOLS men's swimming finishes second, women third at SCAC Championships. Mickey Scharbrough captured the 200-yard breaststroke with a time of 2:06.42, while Belaineh also finished third in the 100-yard fr...</p>
          </div>

          <div class="card">
            <img src="./img/side-mtadams.png" alt="">
            <small class="card-subtitle">ACADEMICS</small>
            <h2 class="title">School of Mountaineering</h2>
            <p class="info">The School of Mountaineering at NOLS University offers a world-class arts education to our students, and serves the Central Texas community by offering a wide selection of events, each year, in performance and visual arts.</p>
          </div>

          <div class="card">
            <img src="./img/side-devilsriver.png" alt="">
            <small class="card-subtitle">STUDENT LIFE</small>
            <h2 class="title">Devils River Expedition</h2>
            <p class="info">Devils River is a 4-day paddling and camping festival at NOLS University. Past performers include: Sleigh Bells, Matt & Kim, Del the Funky Homosapien, Black Joe Lewis and the Honeybears and more.</p>
          </div>

          <div class="card">
            <img src="./img/side-shuksan.png" alt="">
            <small class="card-subtitle">NEW SKILLS</small>
            <h2 class="title">Mount Shuksan Expedtion</h2>
            <p class="info">Mount Shuksan is a 4-day paddling and camping festival at NOLS University. Past performers include: Sleigh Bells, Matt & Kim, Del the Funky Homosapien, Black Joe Lewis and the Honeybears and more.</p>
          </div>

        </div>
      </div> <!-- END .side-content-wrapper -->
    </div>

    <!-- white background that's animated -->
    <div class="nav-content"></div>

    <div class="content" style="background-image: url('./img/bugaboo-spire.jpg')">
      <!-- homepage content -->
      <div class="tagline">
        <h1>Foster the Next Generation. Give Today to Reach Tomorrow.</h1>
        <button class="button-donate">DONATE TO NOLS</button>
      </div>
    </div>
  </div>
</template>

<script>
import anime from 'animejs';

let menuContainer, navContent, menuLinks, primaryMenu, primaryLi, secondaryMenu, secondaryLi, menuLabel, button, logo, cards;

// disabled
// let sideContent

const defaultTiming = 650;
// from Principle
const defaultEasing = [0.25, 0.1, 0.25, 1];

export default {
  name: 'app',
  data () {
    return {
      menuOpen: false
    }
  },
  mounted: function () {
    // set event handlers here since this will
    // be implemented statically
    // set selectors once
    menuContainer = document.querySelector('.menus-container');
    navContent = document.querySelector('.nav-content');
    primaryMenu = document.querySelector('.menu-primary');
    primaryLi = primaryMenu.querySelectorAll('li');
    // firstLink = primaryMenu.querySelector('a');
    secondaryMenu = document.querySelector('.menu-secondary');
    secondaryLi = secondaryMenu.querySelectorAll('li');
    menuLabel = document.querySelector('.menu-label');
    button = menuLabel.querySelector('.button');
    // sideContent = document.querySelector('.side-content-wrapper');
    logo = document.querySelector('.logo');
    cards = document.querySelectorAll('.card');
    menuLinks = document.querySelectorAll('li a');

    // TODO: left menu Gap = 50px;

    menuLinks.forEach(function (d) {
      d.addEventListener('click', function (e) {
        e.preventDefault();
      })
    })
  },
  methods: {
    toggleMenu: function (e) {
      this.menuOpen = !this.menuOpen;
      // disable clicking quickly
      e.target.disabled = true;
      // TODO:
      // should the menu content be hidden or accessible when closed
      menuContainer.classList.toggle('is-hidden');
      if (!this.menuOpen) {
        this.animateSecondary()
      }

      this.animateMenu();
    },
    animateMenu: function () {
      const menuOpacity = this.menuOpen ? 1 : 0;

      // let navTimeline = anime.timeline();
      // Nav container with white background
      anime({
        targets: navContent,
        width: this.menuOpen ? '100%' : 0,
        easing: defaultEasing,
        duration: defaultTiming,
        begin: function (anim) {
          // toggle logo color
          setTimeout(function () {
            logo.classList.toggle('open')
          }, 200)
          // toggle menu icon
          setTimeout(function () {
            menuLabel.classList.toggle('open')
          }, 100)
        }
      })
      // NOTE: setting duration on open only
      //       and resetting to 0 on close
      anime({
        targets: primaryLi,
        elasticity: 0,
        easing: defaultEasing,
        translateX: this.menuOpen ? 50 : 0,
        opacity: menuOpacity,
        delay: function (el, i, l) {
          return i * 90;
        },
        complete: function () {
          // reenable menu button
          button.disabled = false;
        }
      })
    },
    // TODO: opacity & translateX values are not reset
    //       primary is because of click
    animateSecondary: function () {
      const menuOpacity = this.menuOpen ? 1 : 0;

      let secondaryNavTimeline = anime.timeline();

      secondaryNavTimeline
        .add({
          targets: secondaryLi,
          duration: defaultTiming,
          easing: defaultEasing,
          elasticity: 0,
          translateX: this.menuOpen ? [0, 50] : 0,
          opacity: menuOpacity,
          delay: function (el, i, l) {
            return i * 70;
          }
        })
        .add({
          targets: cards,
          elasticity: 0,
          opacity: menuOpacity,
          duration: 1500,
          offset: '-=500',
          translateX: this.menuOpen ? [0, 20] : 0,
          delay: function (el, i, l) {
            return i * 150;
          }
        })
    }
  }
}
</script>

<style lang="sass">
@import url('//fonts.googleapis.com/css?family=Playfair+Display:900');
@import './sass/reset'
@import './sass/app'
@import './sass/sidecontent'
@import './sass/menus'
</style>
