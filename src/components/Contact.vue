<script setup>
    import { ref, onMounted, onBeforeMount } from 'vue';

    import { Notyf } from 'notyf';
    import 'notyf/notyf.min.css';

    const notyf = new Notyf();

    const WEB3FORMS_ACCESS_KEY = "1573cb46-8411-4b44-8e6e-41c95c53b904";

    const subject = "New message from Portfolio Contact Form";

    const name = ref("");
    const email = ref("");
    const message = ref("");

    const isLoading = ref(false);


    const submitForm = async() => {

        if(!recaptchaToken.value) {
            notyf.error('Please verify that you are not a robot.');
            return;
        }

        isLoading.value = true;

        try {

            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json"
                },
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject,
                    name: name.value,
                    email: email.value,
                    message: message.value
                })
            });

            const result = await response.json();

            if(result.success) {
                console.log(result)

                isLoading.value = false;
                notyf.success("Message sent!");
            }

        } catch(error) {
            console.log(error);

            isLoading.value = false;
            notyf.error("Failed to send message");

        } finally {

            resetRecaptcha();
        }
    }

    /*recaptcha integration*/

    const SITE_KEY = '6Lf0K_UsAAAAANNleNQ4vn6kKQOlo4VmVcsqXA7g';

    const recaptchaContainer = ref(null);
    const recaptchaWidgetId = ref(null);
    const recaptchaToken = ref('');


    function onRecaptchaSuccess(token) {
        recaptchaToken.value = token;
    }

    function onRecaptchaExpired() {
        recaptchaToken.value = '';
    }

    function renderRecaptcha() {
        if(!window.grecaptcha) {
            console.error('reCAPTCHA not loaded');
            return;
        }

        recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            size: 'normal',
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired
        });
    }

    function resetRecaptcha() {
        if(recaptchaWidgetId.value !== null) {
            window.grecaptcha.reset(recaptchaWidgetId.value);
            recaptchaToken.value = '';
        }
    }

    onMounted(() => {
        const interval = setInterval(() => {
            if(window.grecaptcha && window.grecaptcha.render) {
                renderRecaptcha();
                clearInterval(interval)
            }
        }, 100);

        onBeforeMount(() => {
            clearInterval(interval);
        });
    })

</script>

<template>
	<!-- Contact Section Start -->
			<div class="row px-5 justify-content-center" id="contact">
				<h1 class="text-center p-5 mt-5 contact-form">CONTACT ME</h1>
				<div class="col-md-6">

						<form class="p-4 border rounded shadow-sm">

							<div class="mb-3">
								<label for="full_name" class="form-label">Full Name:</label>
								<input class="form-control" type="text" name="full_name" id="full_name" placeholder="Enter your name" autocomplete>
							</div>

							<div class="mb-3">
								<label for="email">Email Address:</label>
								<input class="form-control" type="email" name="email" id="email" placeholder="email@example.com">
							</div>

							<div class="mb-3">
								<label for="message" class="form-label">Message:</label>
								<textarea class="form-control" name="message" rows="5" id="message" placeholder="Enter your message"></textarea>
							</div>

							<div class="text-center">
								<button type="button" class="btn btn-primary btn-outline-light p-2">SEND</button>
							</div>
							
						</form>
					
				</div>
			</div>
		<!-- Contact Section End -->

		<!-- Social Media Links Start -->
			<div class="row px-5 justify-content-center">
				<div class="col-md-6 mt-5 text-center social-icons">
					 <a href="https://www.linkedin.com/in/vaneza-diarios/" target="_blank">
					 	<i class="fa-brands fa-linkedin"></i>
					 </a>
					 <a href="https://github.com/vaneza-code" target="_blank">
					 	<i class="fa-brands fa-github"></i>
					 </a>
					 <a href="https://www.instagram.com/binibanee/" target="_blank">
					    <i class="fa-brands fa-instagram"></i>
					 </a>
					 <a href="https://www.facebook.com/diariosvaneza" target="_blank">
					    <i class="fa-brands fa-facebook"></i>
					 </a>
				</div>
			</div>
		<!-- Social Media Links End -->

</template>