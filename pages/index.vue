<template>
    <div class="page">
        <h1>Welcome to UXhouse!</h1>
        <main>
            <div id="content">
                <p>
                    We’re a group of UX enthusiasts and professionals creating
                    a community for human-centered thinkers to collaborate,
                    critique, and share their ideas.
                </p>
                <p>Did we mention waffles? We have waffles!</p>
            </div>
            <form
                name="register"
                method="POST"
                @submit.prevent="register"
                data.netlify="true"
                netlify-honeypot="bot-field"
            >
                <h2>Join us!</h2>
                <input
                    name="form-name"
                    type="hidden"
                    value="register"
                />
                <div class="text-field-wrap">
                    <input
                        :class="[attemptSubmit && !validDiscordUser
                        ? 'text-field--error' : '', 'text-field']"
                        name="discord-user"
                        placeholder="Discord username"
                        type="text"
                        v-model="form.discordUser"
                    />
                    <span
                        class="text-field-message"
                        v-if="attemptSubmit && !validDiscordUser"
                    >
                        Provide a valid discord username.
                    </span>
                </div>
                <div class="text-field-wrap">
                    <input
                        :class="[attemptSubmit && !validEmail
                        ? 'text-field--error' : '', 'text-field']"
                        name="email"
                        placeholder="Email"
                        type="email"
                        v-model="form.email"
                    />
                    <span
                        class="text-field-message"
                        v-if="attemptSubmit && !validEmail"
                    >
                        Provide a valid email address.
                    </span>
                </div>
                <div class="text-field-wrap">
                    <textarea
                        :class="[attemptSubmit && !validInterest
                        ? 'text-field--error' : '', 'text-field']"
                        name="interest"
                        placeholder="Tell us a bit more about your interest in UX..."
                        type="text"
                        v-model="form.interest"
                    />
                    <span
                        class="text-field-message"
                        v-if="attemptSubmit && !validInterest"
                    >
                        The interest field is required.
                    </span>
                </div>
                <button
                    type="submit"
                    class="button-primary"
                >
                    Sign Up
                </button>
            </form>
        </main>
        <div id="waffles">
            <img
                src="@/assets/images/waffle.png"
                alt="Waffle Mascot"
            >
            <img
                src="@/assets/images/waffle.png"
                alt="Waffle Mascot"
            >
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        attemptSubmit: false,
        form: {
            email: '',
            discordUser: '',
            'form-name': 'register',
            interest: ''
        }
    }),

    computed: {
        validEmail() {
            let re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
            return re.test(this.form.email)
        },

        validDiscordUser() {
            let re = /^((.+?)#\d{4})/
            return re.test(this.form.discordUser)
        },

        validInterest() {
            return this.form.interest === '' ? false : true
        },

        validForm() {
            return this.validEmail
            && this.validDiscordUser
            && this.validInterest
        },
    },

    methods: {
        encode(data) {
            return Object.keys(data)
                .map(key => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`)
                .join("&")
        },

        async register() {
            this.attemptSubmit = true
            if (!this.validForm) return

            let response = await fetch(this.$route.path, {
                method: 'POST',
                headers: { 'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8' },
                body: this.encode({ ...this.form })
            })

            let responseOK = response && response.ok

            if (responseOK) return

            throw new Error(response.statusText)
        },
    }
}
</script>

<style lang="scss" scoped>
.page {
    padding: 5vh 5vw;
}

h1 {
    font-size: 52px;
    padding: 0 0 1rem;
}

#content {
    width: 100%;

    p {
        font-size: 20px;

        &:first-child {
            margin: 0 0 1rem;
        }
    }
}

form {
    display: flex;
    flex-flow: column;
    margin: 1rem 0 0;

    h2 {
        margin-bottom: 0.5rem;
    }
}

.text-field-wrap {
    padding: 1rem 0;
    position: relative;
}

.text-field {
    background: #FFFFFF;
    border: 2px solid #000000;
    border-radius: 6px;
    padding: 0.35rem 0.5rem;
    width: 100%;

    &:focus {
        border: 2px solid blue;
        outline: none;
    }

    &--error {
        border: 2px solid red;
    }
}

.text-field-message {
    color: red;
    font-size: 14px;
    position: absolute;
    left: 0;
    bottom: -5px;
}

.button-primary {
    background: #000000;
    color: #FFFFFF;
    border-radius: 6px;
    padding: 0.35rem 0.5rem;
    font-size: 20px;
    margin: 1rem 0;
}

#waffles {
    position: fixed;
    right: -17%;
    bottom: -1%;
    z-index: -1;

    img {
        width: 250px;

        &:first-child {
            position: absolute;
            right: -0px;
            bottom: -45px;
        }

        &:nth-child(2) {
            position: absolute;
            right: -40px;
            bottom: -40px;
        }
    }
}
</style>
