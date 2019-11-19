<template>
    <div class="page">
        <main>
            <h1>Welcome to UXhouse!</h1>
            <div id="content">
                <div id="paragraphs">
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
                    data-netlify="true"
                    netlify-honeypot="bot-field"
                >
                    <h2>Join us!</h2>
                    <input
                        type="hidden"
                        name="form-name"
                        value="register"
                    />
                    <div class="text-field-wrap">
                        <input
                            :class="[attemptSubmit && !validDiscordUser
                            ? 'text-field--error' : '', 'text-field']"
                            name="discord-user"
                            placeholder="Discord ID"
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
                            resize="disabled"
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
                    <span
                        style="color: green;"
                        v-if="submitSuccess"
                    >
                        Submitted. Check your email for the invite!
                    </span>
                    <span
                        style="color: red;"
                        v-if="submitError"
                    >
                        {{ errorMessage }}
                    </span>
                </form>
            </div>
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

<script id="mcjs">!function(c,h,i,m,p){m=c.createElement(h),p=c.getElementsByTagName(h)[0],m.async=1,m.src=i,p.parentNode.insertBefore(m,p)}(document,"script","https://chimpstatic.com/mcjs-connected/js/users/db2c7a4afefa1c05bf294cd7f/5148ad6d1f0796d956de14712.js");</script>

<script>
export default {
    data: () => ({
        attemptSubmit: false,
        errorMessage: '',
        form: {
            email: '',
            discordUser: '',
            'form-name': 'register',
            interest: ''
        },
        submitError: false,
        submitSuccess: false
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

            if (responseOK) {
                this.submitSuccess = true
                return
            }

            this.submitError = true
            this.errorMessage = response.statusText
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
    font-size: 44px;
    padding: 0 0 1rem;
}

#content {
    width: 100%;

}

#paragraphs {
    max-width: 500px;

        p {
            font-size: 18px;
            line-height: 35px;

        &:first-child {
            margin: 0 0 1rem;
        }
    }
}

form {
    display: flex;
    flex-flow: column;
    margin: 1rem 0 0;
    width: 100%;
    max-width: 480px;

    h2 {
        margin-bottom: 0.5rem;
    }

    textarea {
        resize: none;
    }
}

.text-field-wrap {
    padding: 0.5rem 0;
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

@media (min-width: 625px) {
    h1 {
        font-size: 52px;
    }

    .page {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-flow: column;
    }

    main {
        width: 100%;
        max-width: 650px;
        display: flex;
        justify-content: space-between;
        flex-flow: column;
    }

    #content {
        display: flex;
        justify-content: space-between;
    }

    #paragraphs {
        max-width: 360px;

        p {
            font-size: 22px;
        }
    }

    form {
        margin: 0 0 0 1rem;
        max-width: 250px;
    }

    #waffles {
        right: -5%;
        bottom: -5%;
        img {
            width: 400px;
        }
    }
}
</style>
