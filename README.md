<script>
const openForm = document.getElementById("openForm");
const form = document.getElementById("memberForm");
const result = document.getElementById("result");
const cardName = document.getElementById("cardName");
const cardDob = document.getElementById("cardDob");
const backBtn = document.getElementById("backBtn");

openForm.addEventListener("click", () => {
    form.classList.add("show");
    openForm.style.display = "none";

    form.scrollIntoView({
        behavior: "smooth",
        block: "center"
    });
});

form.addEventListener("submit", (e) => {
    e.preventDefault();

    const name = document.getElementById("nom").value.trim();
    const dob = document.getElementById("dob").value;

    if (!name || !dob) return;

    /* الاسم يظهر أمام NOM: */
    cardName.textContent = name.toUpperCase();

    /* تحويل التاريخ إلى DD/MM/YYYY */
    const date = new Date(dob + "T00:00:00");

    cardDob.textContent = new Intl.DateTimeFormat("fr-FR", {
        day: "2-digit",
        month: "2-digit",
        year: "numeric"
    }).format(date);

    /* إخفاء النموذج */
    form.style.display = "none";

    /* إنشاء شاشة التحميل */
    const loader = document.createElement("div");

    loader.id = "loadingScreen";

    loader.innerHTML = `
        <div class="loading-box">
            <div class="loading-title">GLADIATORS</div>

            <div class="loading-percent">0%</div>

            <div class="loading-bar">
                <div class="loading-progress"></div>
            </div>

            <div class="loading-text">
                CRÉATION DE VOTRE MEMBER CARD...
            </div>
        </div>
    `;

    document.body.appendChild(loader);

    const style = document.createElement("style");

    style.innerHTML = `
        #loadingScreen {
            position: fixed;
            inset: 0;
            background: #000;
            z-index: 99999;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 20px;
        }

        .loading-box {
            width: min(500px, 90%);
        }

        .loading-title {
            color: #fff;
            font-size: 35px;
            font-weight: 900;
            letter-spacing: 7px;
            margin-bottom: 35px;
        }

        .loading-percent {
            color: #fff;
            font-size: 50px;
            font-weight: 900;
            margin-bottom: 20px;
        }

        .loading-bar {
            width: 100%;
            height: 8px;
            background: #222;
            border-radius: 20px;
            overflow: hidden;
        }

        .loading-progress {
            width: 0%;
            height: 100%;
            background: #fff;
            transition: width .08s linear;
        }

        .loading-text {
            color: #777;
            font-size: 11px;
            letter-spacing: 2px;
            margin-top: 18px;
        }
    `;

    document.head.appendChild(style);

    const percent = loader.querySelector(".loading-percent");
    const progress = loader.querySelector(".loading-progress");

    let number = 0;

    const loading = setInterval(() => {

        number++;

        percent.textContent = number + "%";
        progress.style.width = number + "%";

        if (number >= 100) {

            clearInterval(loading);

            setTimeout(() => {

                loader.remove();
                style.remove();

                result.classList.add("show");

                result.scrollIntoView({
                    behavior: "smooth",
                    block: "center"
                });

            }, 500);
        }

    }, 30);
});


backBtn.addEventListener("click", () => {

    result.classList.remove("show");

    form.style.display = "block";
    form.classList.add("show");

    form.scrollIntoView({
        behavior: "smooth",
        block: "center"
    });

});
</script>
