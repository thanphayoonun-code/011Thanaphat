
const accounts = {
    admin: {
        username: "admin",
        password: "admin123"
    },
    user: {
        username: "user",
        password: "user123"
    }
};

function updateAuthMenu() {
    const menus = document.querySelectorAll(".auth-menu");

    const loggedIn = sessionStorage.getItem("loggedIn");
    const username = sessionStorage.getItem("username");

    menus.forEach(function(menu) {
        menu.innerHTML = "";

        if (loggedIn === "true" && username) {
            const name = document.createElement("span");
            name.className = "user-name";
            name.textContent = "สวัสดี, " + username;

            const logout = document.createElement("button");
            logout.type = "button";
            logout.className = "logout-button";
            logout.textContent = "ออกจากระบบ";
            logout.addEventListener("click", logoutUser);

            menu.appendChild(name);
            menu.appendChild(logout);
        } else {
            const login = document.createElement("a");
            login.href = "login.html";
            login.className = "login-nav";
            login.textContent = "เข้าสู่ระบบ";
            menu.appendChild(login);
        }
    });
}

function logoutUser() {
    sessionStorage.removeItem("loggedIn");
    sessionStorage.removeItem("username");
    sessionStorage.removeItem("role");

    window.location.href = "index.html";
}

document.addEventListener("DOMContentLoaded", function() {

    updateAuthMenu();

    const loginForm = document.getElementById("loginForm");

 
    if (!loginForm) {
        return;
    }

    const passwordInput = document.getElementById("password");
    const togglePassword = document.getElementById("togglePassword");
    const loginMessage = document.getElementById("loginMessage");

    togglePassword.addEventListener("click", function() {

        if (passwordInput.type === "password") {
            passwordInput.type = "text";
            togglePassword.textContent = "ซ่อน";
        } else {
            passwordInput.type = "password";
            togglePassword.textContent = "แสดง";
        }

    });

    loginForm.addEventListener("submit", function(event) {

        event.preventDefault();

        const username = document
            .getElementById("username")
            .value.trim();

        const password = passwordInput.value;

        const role = document
            .getElementById("role")
            .value;

        const account = accounts[role];

        loginMessage.className = "";

        if (
            account &&
            username === account.username &&
            password === account.password
        ) {

            loginMessage.textContent = "เข้าสู่ระบบสำเร็จ!";
            loginMessage.className = "success";

            sessionStorage.setItem("loggedIn", "true");
            sessionStorage.setItem("username", username);
            sessionStorage.setItem("role", role);

            setTimeout(function() {
                window.location.href = "index.html";
            }, 800);

        } else {

            loginMessage.textContent =
                "ชื่อผู้ใช้ รหัสผ่าน หรือประเภทผู้ใช้ไม่ถูกต้อง";

            loginMessage.className = "error";
        }

    });

});



document.addEventListener("DOMContentLoaded", function () {

    const slides = document.querySelectorAll(".mySlides");
    const dots = document.querySelectorAll(".slider-dots .dot");

    if (slides.length === 0) {
        return;
    }

    let slideIndex = 0;
    let slideTimer;
    function showSlide(index) {

        slideIndex = (index + slides.length) % slides.length;

        slides.forEach(function (slide) {
            slide.style.display = "none";
        });

        dots.forEach(function (dot) {
            dot.classList.remove("active");
            dot.setAttribute("aria-current", "false");
        });

        slides[slideIndex].style.display = "block";
        if (dots[slideIndex]) {
            dots[slideIndex].classList.add("active");
            dots[slideIndex].setAttribute("aria-current", "true");
        }
    }

    function nextSlide() {
        showSlide(slideIndex + 1);
    }

    function startAutoSlide() {
        clearInterval(slideTimer);
        slideTimer = setInterval(nextSlide, 3000);
    }

    dots.forEach(function (dot, index) {

        dot.addEventListener("click", function () {

            showSlide(index);

            startAutoSlide();

        });

    });

    showSlide(0);

    startAutoSlide();

});
