<template>
    <div
        id="app"
        class="bg-black/95 text-white h-screen w-screen py-12 flex flex-col items-center justify-start space-y-12"
    >
        <div class="flex flex-col items-center justify-start h-fit w-full">
            <h1 class="text-4xl font-medium mb-4">Prognoza ☁️</h1>
            <div
                class="flex flex-row items-center justify-center gap-2 text-sm md:text-lg"
            >
                <input
                    type="text"
                    class="bg-neutral-800 text-white rounded-2xl px-4 py-2 outline-none border border-transparent hover:border-green-600 transition-all duration-300 hover:placeholder-green-500"
                    placeholder="Podaj miasto"
                    v-model="query"
                />
                <button
                    class="bg-green-500 text-green-800 font-semibold rounded-2xl px-4 py-2 shadow"
                    @click="getWeather"
                >
                    Szukaj
                </button>
            </div>
        </div>
        <main
            v-if="weather.name"
            class="flex flex-col items-center justify-start h-full w-full space-y-3"
        >
            <h2 class="text-4xl font-medium">
                {{ weather.name }}, {{ weather.sys.country }}
            </h2>
            <p class="text-2xl leading-3 pb-6">
                {{
                    new Date(weather.dt * 1000)
                        .toLocaleDateString("pl-PL", {
                            weekday: "long",
                            year: "numeric",
                            month: "long",
                            day: "numeric",
                        })
                        .charAt(0)
                        .toUpperCase() +
                    new Date(weather.dt * 1000)
                        .toLocaleDateString("pl-PL", {
                            weekday: "long",
                            year: "numeric",
                            month: "long",
                            day: "numeric",
                        })
                        .slice(1)
                }}
            </p>
            <h1 class="text-6xl font-semibold text-green-500">
                {{ Math.floor(weather.main.temp) }}
                <span class="text-white">°C</span>
            </h1>
            <div class="flex items-center justify-center gap-3 text-lg">
                <p class="font-medium">
                    {{
                        weather.weather[0].description.charAt(0).toUpperCase() +
                        weather.weather[0].description.slice(1)
                    }},
                </p>
                <img
                    :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}.png`"
                    alt=""
                />
            </div>
        </main>
    </div>
</template>

<script>
export default {
    name: "app",
    data() {
        return {
            api_key: "c4d2bf29784c20816ff3debb069c1e00",
            base_url: "https://api.openweathermap.org/data/2.5/",
            query: "",
            weather: {},
            exclude: "hourly,alerts, daily",
        };
    },
    methods: {
        async getWeather() {
            if (query != ("" || null)) {
                const res = await fetch(
                    `${this.base_url}weather?q=${this.query}&exclude=${this.exclude}&lang=pl&appid=${this.api_key}&units=metric`
                );
                const data = await res.json();
                this.weather = data;
            }
        },
    },
};
</script>
