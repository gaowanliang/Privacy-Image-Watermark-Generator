<template>
    <n-card class="card" :title="contentText.formTitle">
        <n-form ref="formRef" label-width="auto" :model="formValue" label-position="top" size="large" class="form">
            <!-- Issue 标题 -->
            <n-form-item :label="contentText.inputTextTitle" path="text">
                <n-dynamic-input v-model:value="formValue.text" :placeholder="contentText.inputTextTitleHint" :min="1"
                    :max="3" />
            </n-form-item>
            <n-space>
                <n-form-item :label="contentText.watermarkWidth" path="inputNumberValue">
                    <n-input-number v-model:value="formValue.width" />
                </n-form-item>
                <n-form-item :label="contentText.watermarkHeight" path="inputNumberValue">
                    <n-input-number v-model:value="formValue.height" />
                </n-form-item>
            </n-space>
            <n-space style="padding-bottom: 15px ;">{{ contentText.grandientColor }}<n-switch
                    v-model:value="formValue.multiColors" /></n-space>

            <n-form-item :label="contentText.colorSelect" path="colorValue">
                <n-color-picker v-model:value="formValue.colorValue" />
            </n-form-item>
            <div v-if="formValue.multiColors === true">
                <n-form-item :label="contentText.grandientColor2Select" path="colorValue2">
                    <n-color-picker v-model:value="formValue.colorValue2" />
                </n-form-item>
                <n-form-item :label="contentText.grandientColor3Select" path="colorValue3">
                    <n-color-picker v-model:value="formValue.colorValue3" />
                </n-form-item>
            </div>
            <n-space style="padding-bottom: 15px ;">{{ contentText.advancedSettings }}<n-switch
                    v-model:value="formValue.advanced" /></n-space>
            <div v-if="formValue.advanced === true">
                <n-form-item :label="contentText.rotato" path="inputNumberValue">
                    <n-input-number v-model:value="formValue.rotate" />
                </n-form-item>
                <n-form-item :label="contentText.spacing" path="spacing">
                    <n-input v-model:value="formValue.spacing" />
                </n-form-item>
                <n-form-item :label="contentText.fontSizeAndFamily" path="fontSizeAndType">
                    <n-input v-model:value="formValue.fontSizeAndType" />
                </n-form-item>

            </div>
        </n-form>
        <div style="display: flex; justify-content: flex-end">
            <n-button type="primary" @click="changeWatermark" style="margin-bottom: 15px">{{ contentText.generateWatermark
            }}</n-button>
        </div>

        <n-card :title="contentText.watermarkPreview" size="small">

            <n-image id="watermarkPreview" :src="imageSrc"></n-image>

        </n-card>
    </n-card>
    <n-card class="card" :title="contentText.uploadImage">
        <n-upload directory-dnd :max="1" @before-upload="beforeUpload">
            <n-upload-dragger>
                <div style="margin-bottom: 12px">
                    <n-icon size="48" :depth="3">
                        <archive-icon />
                    </n-icon>
                </div>
                <n-text style="font-size: 16px">
                    {{ contentText.uploadImageHint }}
                </n-text>

            </n-upload-dragger>
        </n-upload>
        <div style="display: flex; padding-top: 15px ; justify-content: flex-end">
            <n-button type="primary" @click="watermarkingImg" style="margin-bottom: 15px">{{ contentText.watermarkingImage
            }}</n-button>
        </div>
    </n-card>
    <n-card class="card" :title="contentText.doneImage">
        <n-layout>
            <n-image :src="doneImageSrc"></n-image>
        </n-layout>
        <h5>
            {{ contentText.saveHint }}
        </h5>
        <div style="display: flex; padding-top: 15px ; justify-content: flex-end" v-if="doneImageSrc !== ''">
            <n-button type="primary" @click="downloadImage" style="margin-bottom: 15px">
                <template #icon>
                    <n-icon>
                        <cloud-download />
                    </n-icon>
                </template>

                {{ contentText.downloadImage
                }}</n-button>
        </div>
    </n-card>
    <n-card class="card" :title="contentText.isSafty">
        {{ contentText.saftyClaim1 }}<a href="https://github.com/gaowanliang/Privacy-Image-Watermark-Generator"
            target="_blank">GitHub</a>{{ contentText.saftyClaim2 }}
    </n-card>
    <h5 style="text-align: center">
        <a href="https://github.com/gaowanliang" target="_blank">Gaowan Liang</a> © 2023 - {{ new Date().getFullYear() }} ,
        All Rights Reserved
    </h5>
</template>

<script lang="ts">
import { defineComponent, computed, toRef, ref } from 'vue'
import type { PropType } from 'vue'
import content from '../content/content'
import { ArchiveOutline as ArchiveIcon, CloudDownload } from '@vicons/ionicons5'

import {
    NCard,
    NForm,
    NFormItem,
    NInput,
    NImage,
    NDynamicInput,
    NInputNumber,
    NButton,
    NSpace,
    NSwitch,
    NColorPicker,
    NUpload,
    NUploadDragger,
    NIcon,
    NText,
    NLayout,
    useMessage,
    type UploadFileInfo,
    type FormInst
} from 'naive-ui'

// # 000000FF to rgba(0,0,0,1)
function hexToRgbA(hex: string) {
    // remove # if it exists
    hex = hex.replace('#', '');
    var r = parseInt(hex.substring(0, 2), 16);
    var g = parseInt(hex.substring(2, 4), 16);
    var b = parseInt(hex.substring(4, 6), 16);
    var a = parseInt(hex.substring(6, 8), 16) / 255;
    // a 保留两位小数
    a = Math.round(a * 100) / 100
    return "rgba(" + r + ", " + g + ", " + b + ", " + a + ")";
}


export default defineComponent({
    name: 'IssuePage',
    props: {
        lang: {
            type: String as PropType<'en-US' | 'zh-CN'>
        }
    },

    components: {
        NForm,
        NFormItem,
        NInput,
        NInputNumber,
        NDynamicInput,
        NCard,
        NImage,
        NSpace,
        NSwitch,
        NColorPicker,
        NUpload,
        NUploadDragger,
        NIcon,
        NButton,
        NLayout,
        NText,
        ArchiveIcon,
        CloudDownload,
    },
    methods: {},
    setup: (props) => {
        const langs = toRef(props, 'lang')
        const contentText = computed(() => content[langs.value ?? 'en-US'])
        const formRef = ref<FormInst | null>(null)
        const formValue = ref({
            text: [''],
            height: 300,
            width: 40,
            multiColors: false,
            colorValue: '#00000066',
            colorValue2: '#c0392b66',
            colorValue3: '#16a08566',
            advanced: false,
            fontSizeAndType: "20px Microsoft Yahei",
            rotate: -40,
            spacing: "2.9,2.4,2.0"
        })
        const imageSrc = ref('')
        const changeWatermark = () => {
            var tempCanvas = document.createElement('canvas')
            var tempCtx = tempCanvas.getContext('2d')
            var cw, ch
            cw = formValue.value.width
            ch = formValue.value.height
            tempCtx!.rotate((Math.PI / 180) * formValue.value.rotate)
            tempCtx!.font = formValue.value.fontSizeAndType

            if (formValue.value.multiColors === true) {
                var gradient = tempCtx!.createLinearGradient(0, 0, cw, 0)
                gradient.addColorStop(0, hexToRgbA(formValue.value.colorValue))
                console.log(formValue.value.colorValue, hexToRgbA(formValue.value.colorValue))
                gradient.addColorStop(0.5, hexToRgbA(formValue.value.colorValue2))
                gradient.addColorStop(1, hexToRgbA(formValue.value.colorValue3))
                tempCtx!.fillStyle = gradient
            } else {
                tempCtx!.fillStyle = hexToRgbA(formValue.value.colorValue)
            }

            tempCtx!.textAlign = 'center'
            tempCtx!.textBaseline = 'middle'

            console.log(formRef.value)
            var spacing = formValue.value.spacing.split(',')
            tempCtx!.fillText(formValue.value.text[0], cw / 10, ch / parseFloat(spacing[0]))
            if (formValue.value.text[1]) {
                tempCtx!.fillText(formValue.value.text[1], cw / 10, ch / parseFloat(spacing[1]))
            }
            if (formValue.value.text[2]) {
                tempCtx!.fillText(formValue.value.text[2], cw / 10, ch / parseFloat(spacing[2]))
            }
            imageSrc.value = tempCanvas.toDataURL('image/png')
        }

        const message = useMessage()
        var fileListRef: UploadFileInfo | undefined = undefined;
        const doneImageSrc = ref('')

        function watermarkingImg() {
            console.log(fileListRef)
            if (typeof fileListRef === 'undefined') {
                message.error(content[langs.value ?? 'en-US'].plsUploadImgFirst)
                return
            }
            if (formValue.value.text[0] === '') {
                message.error(content[langs.value ?? 'en-US'].plsInputWatermarkTextFirst)
                return
            }
            if (imageSrc.value === '') {
                message.error(content[langs.value ?? 'en-US'].plsGenerateWatermarkFirst)
                return
            }

            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');

            const fr = new FileReader();
            const { file } = fileListRef
            fr.readAsDataURL(file as File);
            fr.onload = function () {

                var img = new Image();
                console.log(fr.result as string)
                img.src = fr.result as string;
                img.onload = function () {
                    canvas.width = img.width;
                    canvas.height = img.height;
                    ctx!.drawImage(img, 0, 0);
                    var watermark = new Image();
                    watermark.src = imageSrc.value;
                    ctx!.fillStyle = ctx!.createPattern(watermark, 'repeat') as CanvasPattern;
                    ctx!.fillRect(0, 0, canvas.width, canvas.height);
                    // console.log(canvas.toDataURL('image/png'))

                    doneImageSrc.value = canvas.toDataURL('image/png')
                }
            }
        }
        function downloadImage() {
            var a = document.createElement('a');
            a.href = doneImageSrc.value;
            a.download = 'watermark.png';
            a.click();
        }

        return {
            contentText,
            formRef,
            formValue,
            imageSrc,
            changeWatermark,
            watermarkingImg,
            doneImageSrc,
            downloadImage,


            async beforeUpload(data: {
                file: UploadFileInfo
                fileList: UploadFileInfo[]
            }) {
                if (data.file.file?.type !== 'image/png' && data.file.file?.type !== 'image/jpeg') {
                    message.error(content[langs.value ?? 'en-US'].onlyCanUploadImage)
                    return false
                }
                fileListRef = data.file
                return true
            }
        }
    }
})
</script>

<style scoped>
.card {
    margin-bottom: 24px;
}

.form {
    margin-top: 10px;
}

.m-b-24 {
    margin-bottom: 24px;
}

.preview {
    margin-bottom: 10px;
    display: flex;
    justify-content: center;
}

.preview-content {
    word-wrap: break-word;
    word-break: normal;
    overflow: hidden;
}

.preview-footer {
    width: 100%;
    display: flex;
    justify-content: center;
}

.n-iamge img {
    width: 100%;
}
</style>
