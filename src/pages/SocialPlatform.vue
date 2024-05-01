<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import { useStore } from 'vuex'
import axios from 'axios';
import Sidebar from '../components/Sidebar.vue'
import FIleHandler from '../utils/fileHandler'

const store = useStore()
const title = ref('Social Platform')
const subTitle = ref('社交平台')
const message = ref('請輸入配對雙方的腦波資料')
const currentData = computed(() => store.getters.currentData)
const fileHandler = new FIleHandler()


watch(currentData, (newVal, oldVal) => {
    if (newVal) {
    }
})

onMounted(() => {
    if (currentData.value) {
        // fetchMusicXML(currentData.value)
    }
})

const readFile = (file) => {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = () => {
            resolve(reader.result);
        };
        reader.onerror = (error) => {
            reject(error);
        };
        reader.readAsText(file);
    });
};

const parseBrainData = (data) => {
    const dataLength = data['attention'].value.length;
    const placeholder = Array(dataLength).fill(100);
    return {
        "Good Signal Quality(0-100)": placeholder,
        "Attention": data['attention'].value,
        "Meditation": data['meditation'].value,
        "Delta": data['delta'].value,
        "Theta": data['theta'].value,
        "Low Alpha": data['lowAlpha'].value,
        "High Alpha": data['highAlpha'].value,
        "Low Beta": data['lowBeta'].value,
        "High Beta": data['highBeta'].value,
    }
};

const handleFiles = async () => {
    const fileInputs = document.querySelectorAll('input[type="file"]');
    const filesData = {};
    const promises = [];

    for (const fileInput of fileInputs) {
        const file = fileInput.files[0];
        if (file) {
            const promise = readFile(file).then(data => {
                filesData[fileInput.name] = fileHandler.parseCsvData(data);
            });
            promises.push(promise);
        }
    }

    // Wait for all files to be read and parsed
    await Promise.all(promises);
    const payload = preparePayload(filesData);
    console.log(payload);
};

const preparePayload = (filesData) => {
    const payload = {
        "first_person": {
            "first_test": {
                "beforeBrainData": parseBrainData(filesData['user1-pre-before-data']),
                "afterBrainData": parseBrainData(filesData['user1-pre-after-data'])
            },
            "second_test": {
                "beforeBrainData": parseBrainData(filesData['user1-post-before-data']),
                "afterBrainData": parseBrainData(filesData['user1-post-after-data'])
            }
        },
        "second_person": {
            "first_test": {
                "beforeBrainData": parseBrainData(filesData['user2-pre-before-data']),
                "afterBrainData": parseBrainData(filesData['user2-pre-after-data'])
            },
            "second_test": {
                "beforeBrainData": parseBrainData(filesData['user2-post-before-data']),
                "afterBrainData": parseBrainData(filesData['user2-post-after-data'])
            }
        }
    };
    return payload;
};

</script>
<template>
    <div class="p-4 w-full max-w-7xl pb-8">
        <div class="grid grid-cols-1 lg:grid-cols-4 space-y-4 lg:gap-6 lg:space-y-0">
            <div class="flex flex-col w-full">
                <div class="card p-2 bg-base-100 shadow-xl">
                    <Sidebar />
                    <form class="flex flex-col space-y-2" @submit.prevent="">
                        <div class="space-y-2">
                            <h3 class="text-lg font-bold">配對後數據</h3>
                            <div class="p-2 border rounded-md">
                                <h4 class="font-xl font-bold">第一人</h4>
                                <label for="user1-pre-before-data">前測資料</label>
                                <input type="file" required name="user1-pre-before-data" accept=".csv" />
                                <label for="user1-pre-after-data">後側資料</label>
                                <input type="file" required name="user1-pre-after-data" accept=".csv" />
                            </div>
                            <div class="p-2 border rounded-md">
                                <h4 class="font-xl font-bold">第二人</h4>
                                <label for="user1-pre-before-data">前測資料</label>
                                <input type="file" required name="user2-pre-before-data" accept=".csv" />
                                <label for="user2-pre-after-data">後側資料</label>
                                <input type="file" required name="user2-pre-after-data" accept=".csv" />
                            </div>
                        </div>
                        <div class="divider"></div>
                        <div class="space-y-2">
                            <h3 class="text-lg font-bold">配對後數據</h3>
                            <div class="p-2 border rounded-md">
                                <h4 class="font-xl font-bold">第一人</h4>
                                <label for="user1-post-before-data">前測資料</label>
                                <input type="file" required name="user1-post-before-data" accept=".csv" />
                                <label for="user1-post-after-data">後側資料</label>
                                <input type="file" required name="user1-post-after-data" accept=".csv" />
                            </div>
                            <div class="p-2 border rounded-md">
                                <h4 class="font-xl font-bold">第二人</h4>
                                <label for="user2-post-before-data">前測資料</label>
                                <input type="file" required name="user2-post-before-data" accept=".csv" />
                                <label for="user2-post-after-data">後側資料</label>
                                <input type="file" required name="user2-post-after-data" accept=".csv" />
                            </div>
                        </div>
                        <button class="btn btn-primary" type="submit" @click="handleFiles">Upload</button>
                    </form>
                </div>
            </div>
            <div class="md:col-span-3" ref="targetElement">

                <div class="w-full">
                    <div class="flex flex-col gap-6">
                        <div class="card bg-base-100 shadow-xl">
                            <div class="card-body">
                                <div class="flex w-full justify-start items-center">
                                    <header class="flex flex-col justify-start">
                                        <h2 class="text-2xl md:text-4xl font-semibold">{{ title }}</h2>
                                        <h3 class="text-md">{{ subTitle }}</h3>
                                    </header>
                                </div>
                                <div class="text-sm font-semibold">{{ message }}</div>
                                <div v-show="currentData">
                                    <div class="space-y-4">
                                        <div class="flex flex-col space-y-2 space-x-2 justify-between">
                                            <div class="flex space-x-2 justify-between items-end">
                                            </div>
                                        </div>
                                        <div class="divider"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped></style>