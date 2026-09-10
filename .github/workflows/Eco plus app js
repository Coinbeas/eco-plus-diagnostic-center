"use strict";

/*
 * Diagnostic Admin Frontend
 *
 * IMPORTANT:
 * This file is a frontend demo.
 *
 * Real authentication must be performed by a backend.
 * Never put:
 *   - passwords
 *   - database credentials
 *   - TOTP secrets
 *   - private API keys
 *
 * directly in this JavaScript file.
 */


const loginPage = document.getElementById("loginPage");
const twoFactorPage = document.getElementById("twoFactorPage");
const dashboardPage = document.getElementById("dashboardPage");

const loginForm = document.getElementById("loginForm");
const twoFactorForm = document.getElementById("twoFactorForm");

const loginMessage = document.getElementById("loginMessage");
const twoFactorMessage =
    document.getElementById("twoFactorMessage");

const backToLogin =
    document.getElementById("backToLogin");

const logoutBtn =
    document.getElementById("logoutBtn");

const clearActivity =
    document.getElementById("clearActivity");

const activityList =
    document.getElementById("activityList");


/*
 * Page navigation
 */

function showPage(page) {

    document
        .querySelectorAll(".page")
        .forEach(item => item.classList.remove("active"));

    page.classList.add("active");
}


/*
 * Messages
 */

function showMessage(element, text, type = "") {

    element.textContent = text;
    element.className = "message";

    if (type) {
        element.classList.add(type);
    }
}


/*
 * Demo login
 *
 * In production:
 *
 * fetch("/api/admin/login", {
 *     method: "POST",
 *     headers: {"Content-Type": "application/json"},
 *     body: JSON.stringify(...)
 * });
 */

loginForm.addEventListener("submit", function (event) {

    event.preventDefault();

    const email =
        document.getElementById("email").value.trim();

    const password =
        document.getElementById("password").value;

    if (!email || !password) {
        showMessage(
            loginMessage,
            "Please enter your email and password.",
            "error"
        );

        return;
    }

    /*
     * Demo behavior:
     * credentials are not actually checked here.
     *
     * A real backend should validate them.
     */

    showMessage(
        loginMessage,
        "Password accepted. 2FA verification required.",
        "success"
    );

    setTimeout(() => {

        showPage(twoFactorPage);

        document
            .getElementById("totpCode")
            .focus();

    }, 500);
});


/*
 * TOTP verification
 *
 * A real application must verify TOTP
 * on the server using the admin's secret.
 */

twoFactorForm.addEventListener("submit", function (event) {

    event.preventDefault();

    const code =
        document
            .getElementById("totpCode")
            .value
            .trim();

    if (!/^\d{6}$/.test(code)) {

        showMessage(
            twoFactorMessage,
            "Enter a valid 6-digit authentication code.",
            "error"
        );

        return;
    }

    /*
     * Demo only:
     *
     * Any valid 6-digit format is accepted.
     *
     * DO NOT use this as real authentication.
     */

    sessionStorage.setItem(
        "adminAuthenticated",
        "true"
    );

    showMessage(
        twoFactorMessage,
        "2FA verified successfully.",
        "success"
    );

    setTimeout(() => {

        showPage(dashboardPage);

        addActivity(
            "Administrator signed in successfully"
        );

    }, 500);
});


/*
 * Back to login
 */

backToLogin.addEventListener("click", function () {

    document
        .getElementById("totpCode")
        .value = "";

    showMessage(twoFactorMessage, "");

    showPage(loginPage);
});


/*
 * Logout
 */

logoutBtn.addEventListener("click", function () {

    sessionStorage.removeItem(
        "adminAuthenticated"
    );

    document
        .getElementById("password")
        .value = "";

    document
        .getElementById("totpCode")
        .value = "";

    showMessage(loginMessage, "");
    showMessage(twoFactorMessage, "");

    showPage(loginPage);
});


/*
 * Quick actions
 */

document
    .querySelectorAll(".action-btn")
    .forEach(button => {

        button.addEventListener("click", function () {

            const action =
                this.dataset.action;

            const labels = {
                patient: "Patients section selected.",
                appointment:
                    "Appointments section selected.",
                report:
                    "Reports section selected.",
                settings:
                    "Settings section selected."
            };

            addActivity(
                labels[action] || "Action selected."
            );

        });

    });


/*
 * Activity log
 */

function addActivity(text) {

    const item =
        document.createElement("div");

    item.className = "activity";

    const dot =
        document.createElement("span");

    dot.className = "activity-dot";

    const wrapper =
        document.createElement("div");

    const strong =
        document.createElement("strong");

    strong.textContent = text;

    const small =
        document.createElement("small");

    small.textContent = "Just now";

    wrapper.appendChild(strong);
    wrapper.appendChild(small);

    item.appendChild(dot);
    item.appendChild(wrapper);

    activityList.prepend(item);
}


clearActivity.addEventListener("click", function () {

    activityList.innerHTML = "";

});


/*
 * Prevent accidental non-numeric TOTP input
 */

document
    .getElementById("totpCode")
    .addEventListener("input", function () {

        this.value =
            this.value
                .replace(/\D/g, "")
                .slice(0, 6);

    });


/*
 * Initial state
 *
 * Always start at login in this demo.
 */

showPage(loginPage);
