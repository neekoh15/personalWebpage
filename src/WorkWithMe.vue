<template>
  <div class="success-bg"></div>
  <div class="workWithMe">
    <h3>If you reached this far and loved my content ..</h3>
    <h1>Lets <b>work together!</b></h1>
    <div class="contactCard">
      <div class="successMsg">
        <h2>Yay!</h2>
        <div>
          Thank you <b>{{ contactName }}</b
          >, i have recieved your message succesfully! I'll be in touch with you
          as soon as possible. You can also try to contact me throguht my
          linkedin profile
        </div>

        <div>
          <button @click="closePopUp($event)">OK</button>
        </div>
      </div>

      <div class="failedMsg">
        <h2>Ups!</h2>
        <div>
          Hey <b>{{ contactName }}</b
          >, seems like something wen a litle bit wrong! Your message hasnt been
          sended. You can try to contacting me throguht my
          <b>linkedin profile</b> , or mail me at
          <b>dev.nicomartinez@gmail.com</b>
        </div>

        <div>
          <button @click="closePopUp($event)">OK</button>
        </div>
      </div>

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
        <form
          action="https://formspree.io/f/xrgnblzr"
          method="POST"
          id="myForm"
        >
          <div class="cardheader">
            <h3>Get in touch!</h3>
            <div class="success">sent !</div>
            <div class="failure">failed :(</div>
          </div>

          <input
            type="text"
            id="name"
            name="name"
            v-model="contactName"
            required
            placeholder="Your name"
          />
          <input
            type="email"
            id="email"
            name="email"
            required
            placeholder="Your email"
          />
          <input
            type="text"
            id="subject"
            name="subject"
            required
            placeholder="Subject"
          />
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

  data() {
    return {
      contactName: "",
    };
  },

  methods: {
    closePopUp(event) {
      const popUp = event.target.parentNode.parentNode;
      console.log(popUp);
      const bg = document.querySelector(".success-bg");

      popUp.style.display = "none";
      bg.style.display = "none";
    },
  },

  mounted() {
    document.getElementById("myForm").addEventListener("submit", (e) => {
      e.preventDefault();

      let form = e.target;
      let data = new FormData(form);

      /*for (let [key, value] of data.entries()) {
        console.log(key, value);
      }*/
      console.log(data);
      console.log(this.contactName);

      const SucessPopUp = document.querySelector(".successMsg");
      const failedPopUp = document.querySelector(".failedMsg");
      const bg = document.querySelector(".success-bg");
      failedPopUp.style.display = "none";
      SucessPopUp.style.display = "flex";
      bg.style.display = "flex";

      /*fetch(form.action, {
      method: form.method,
      body: data,
      headers: {
        'Accept': 'application/json'
      }
    })
    .then(response => {
      if (response.ok) {
        form.reset(); // Reset the form if you want
        document.getElementById('successMessage').style.display = 'block';
      } else {
        response.json().then(data => {
          if (data.errors) {
            // Display the errors
            console.log(data.errors);
          }
        });
      }
    })
    .catch(error => {
      console.error('Error:', error);
    });
  });*/
    });
  },
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

.success-bg {
  position: fixed;
  display: none;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.8);
  width: 100vw;
  height: 100vh;
}
.successMsg,
.failedMsg {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  position: fixed;
  display: none;
  z-index: 4;

  height: 30vh;
  width: 45%;
  padding: 2rem;
  flex-direction: column;
  gap: 1rem;
  border-radius: 1rem;
  box-shadow: -15px 15px 0px 0px #fff;
}

.successMsg {
  background-color: #98e8cd;
  color: #2c3e50;
}

.failedMsg {
  background-color: orangered;
}

.cardheader {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 2rem;
  height: 2rem;
}

.cardheader h3 {
  display: flex;
  align-items: center;
  justify-content: center;
}

.cardheader div.success {
  background-color: 	#28a745;
  border-radius: 1rem;
  color: white;
  width: 5rem;
  display: flex;
  justify-content: center;
  height: 1.9rem;

}

.cardheader div.failure {
  background-color: orangered;
  border-radius: 1rem;
  color: white;
  width: 6rem;
  display: flex;
  justify-content: center;
  height: 1.9rem;

}
</style>
