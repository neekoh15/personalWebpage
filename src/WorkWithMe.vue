<template>
  <div class="workWithMe">
    <h3>If you reached this far and loved my content ..</h3>
    <h1>Lets <b>work together!</b></h1>
    <div class="contactCard">
      <div class="description">
        <h2>Contact me</h2>
        <p>
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Recusandae
          sequi, ipsa hic alias officia facilis eveniet, neque laborum cumque
          maxime soluta. Neque atque necessitatibus ipsam sequi soluta magni,
          iste vero fuga inventore, explicabo totam quis quia nemo possimus
          cupiditate doloribus? Quasi optio numquam pariatur amet laudantium,
          dicta ullam obcaecati nihil eveniet et aperiam beatae illum vitae in
          asperiores, a ex. Voluptates accusantium, beatae dolorem natus
          delectus nostrum, iusto nemo optio quas eum, quis a in reprehenderit
          totam, sequi distinctio impedit. Dolor aliquid, commodi vitae dolorum
          veniam amet error! Nemo nam quibusdam delectus dolore eveniet officia
          harum eum sit magnam possimus. Eum optio dolores repellat ea impedit,
          in unde, rem ipsam rerum autem iusto vero facilis blanditiis explicabo
          sapiente consequatur provident?
        </p>
        <b> info@mywebsite.com</b>

        <div></div>
      </div>

      <div class="emailCard">
        <form action="https://formspree.io/f/xrgnblzr" method="POST">
          <h3>Get in touch!</h3>
          <input type="text" id="name" name="name" required placeholder="Your name" />
          <input type="email" id="email" name="email" required placeholder="Your email" />
          <input type="text" id="subject" name="subject" required placeholder="Subject" />
          <textarea placeholder="Message" name="message" required></textarea>

          <button type="submit">SEND MESSAGE</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WorkWithMe",
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.workWithMe {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: left;
  text-align: left;
  width: 100%;
}

.welcomeMsg b {
  color: #98e8cd;
}

button {
  height: 3rem;
  width: 100%;
  border: none;
  background-color: #2c3e50;
  color: white;
  border-radius: 0.3rem;
  text-decoration: none;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  cursor: pointer;
}

.contactCard {
  margin-top: 2rem;
  display: flex;
  gap: 2rem;
  justify-content: space-between;
}

.description {
  display: flex;
  gap: 1rem;
  flex-direction: column;
  width: 50%;
}

.emailCard {
  background-color: white;
  border-radius: 0.3rem;
  width: 35%;
  padding: 2rem;
  color: #2c3e50;
  box-shadow: -15px 15px 0px 0px #98e8cd;
}

form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

input,
textarea {
  min-height: 3rem;
  padding-left: 1rem;
  border-radius: 0.3rem;
  border: 1px solid #ccc;
  outline: none;
  font-family: "Hagrid-Regular";
}

textarea {
  min-height: 6rem;
  padding-top: 1rem;
}

p {
  font-weight: 200;
}
</style>
