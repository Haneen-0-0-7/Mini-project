<template>
  <div id="container-whole">
    <Headerabout />
    <title>About Exam Cell Automation</title>
    <div class="container">
      <div class="content-wrapper">
        <div class="has-text-centered">
          <h1 class="about-us-heading">
            <b><u>About Us</u></b>
          </h1>
        </div>
        <div class="about-us-block1-wrapper">
          <div class="about-us-block1">
            <h2><em>AUTOMIZE - Exam Cell Automation</em></h2>
            <p>
              Thank you for visiting Automize, your reliable partner for
              streamlining and improving exam administration procedures. Our
              state-of-the-art automation solutions are created to streamline
              difficult processes and increase management efficiency for exam
              cells. We offer comprehensive software solutions suited to the
              particular requirements of educational institutions, with an
              emphasis on user-friendly interfaces and powerful functionality.
              Our technologies enable institutions to increase efficiency and
              make data-driven decisions across a range of functions, including
              managing exam schedules, student registration, seat allocation,
              and communication. To learn how Automize may revolutionize your
              exam administration operations and boost success in your
              institution, get in touch with us right away.
            </p>
          </div>
          <div class="about-us-block1">
            <img src="../assets/Automiseaboutpic.png" alt="image2" />
          </div>
        </div>
      </div>
      <footer class="footer">
        <div class="content has-text-centered">
          <p>&copy; 2023 Your Company. All rights reserved.</p>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
import Headerabout from "../components/Headerabout.vue";

export default {
  name: "about_",
  components: {
    Headerabout,
  },
};
</script>

<style scoped>
h1 {
  margin: 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.section {
  margin-bottom: 40px;
}

h2 {
  font-size: 24px;
  margin-bottom: 10px;
}

p {
  font-size: 16px;
  line-height: 1.5;
}

a {
  color: #007acc;
}

.btn {
  display: inline-block;
  padding: 10px 20px;
  background-color: #007acc;
  color: white;
  text-decoration: none;
  border-radius: 5px;
  margin-top: 20px;
  transition: background-color 0.3s ease;
}

.btn:hover {
  background-color: #005073;
}
.content-wrapper {
  margin-top: 2em;
}

.has-text-centered {
  margin-bottom: 1.5em;
}

.about-us-heading {
  font-size: 28px;
  text-align: center;
  text-decoration: underline;
  color:white;
}

.about-us-block1-wrapper {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  align-items: center;
  color:white;
}

.about-us-block1 {
  width: 50%;
  text-align: justify;
  margin-right: 1em;
}

#container-whole {
  background-image: linear-gradient(rgba(15, 15, 5, 0.9), rgba(15, 15, 5, 0.9)),
    url("../images/logo.png");
  background-position: center;
  position: relative;
  background-size: cover;
  margin: 0;
}

.about-us-block1 p {
  margin-bottom: 1em;
}

.about-us-block1 img {
  width: 100%;
  height: auto;
}

.footer {
  margin-top: 2em;
  background-color: #fff;
  padding: 1em;
  text-align: center;
}

.footer p {
  margin: 0;
}
</style>
