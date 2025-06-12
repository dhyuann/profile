<template>
    <div class="social-links" :class="props.class">
        <!-- Link -->
        <a v-for="link in parsedLinks"
           class="btn social-link"
           :class="classList"
           @click="goHref(link.href)"
           target="_blank">
            <!-- ToolTip -->
            <div v-if="link.label"
                 class="social-link-tooltip text-1">
                {{link.label}}
            </div>
            <!-- Icon -->
            <i :class="link.faIcon"/>
        </a>
    </div>
    <Modal id="project-modal"
           modalType="modal-xl fade"
           dialogType="modal-dialog-centered"
           :visible="Boolean(showWechat)"
           :dismissable="true"
           @close="onClose"
           >
        <div class="wechat-dialog">
            <img :src="utils.resolvePath(path)" style="width: 400px;height: auto">
        </div>
    </Modal>
</template>

<script setup>
import {computed, inject, reactive, ref} from "vue"
import Modal from "/src/vue/components/modals/base/Modal.vue"
import {useUtils} from "/src/composables/utils.js"


const props = defineProps({
    items: Array,
    size: String,
    variant: String,
    class: String
})
const utils = useUtils()
let path = 'images/flags/wechat.jpg'
/** @type {Function} */
const localizeFromStrings = inject("localizeFromStrings")

const parsedLinks = computed(() => {
    return props.items.map(item => ({
        href: item.href || "/",
        label: localizeFromStrings(item.id).replace("locales.", ""),
        faIcon: item.faIcon || "fa-solid fa-eye"
    }))
})

const classList = computed(() => {
    const sizeClass = "social-link-size-" + props.size
    const colorClass = "social-link-color-" + props.variant
    return sizeClass + " " + colorClass
})

let showWechat = ref(false);
function showWechatImg(){
    showWechat.value = true
}
function onClose(){
    showWechat.value = false
}
function goHref(href){
    if(href != '/'){
        // 创建一个新的窗口对象
        let newWindow = window.open();
        // 在新窗口加载指定链接
        newWindow.location.href = href;
    }else{
        showWechatImg();
    }
   
}
</script>

<style lang="scss" scoped>
@import "/src/scss/_theming.scss";

@mixin create-btn-social-size($std-font-size, $lg-down-font-size, $md-down-font-size) {
    $font-size: $std-font-size;

    font-size: $font-size;
    width: calc($font-size * 2.4);
    height: calc($font-size * 2.4);

    @include media-breakpoint-down(lg) {
        font-size: $lg-down-font-size;
        width: calc($lg-down-font-size * 2.4);
        height: calc($lg-down-font-size * 2.4);
    }

    @include media-breakpoint-down(md) {
        font-size: $md-down-font-size;
        width: calc($md-down-font-size * 2.4);
        height: calc($md-down-font-size * 2.4);
    }
}

@mixin create-btn-social-color($color, $bg-color, $border-color, $hover-color, $hover-bg-color, $hover-border-color) {
    color: $color;
    background-color: $bg-color;
    border-color: $border-color;

    &:hover {
        color: $hover-color;
        background-color: $hover-bg-color;
        border-color: $hover-border-color;
    }
}

a.social-link {
    display: inline-flex;
    justify-content: center;
    align-items: center;

    margin-left: 0.25rem;
    margin-right: 0.25rem;

    border-radius: 100%;
    border-width: 3px;
    color: $white;

    @include create-btn-social-size(1.2rem, 1.1rem, 1rem);
    @include create-btn-social-color(
        $white, lighten($primary, 10%), lighten($primary, 25%),
        $white, lighten($dark, 15%), lighten($dark, 25%),
    )
}

a.social-link-size-2 {
    @include create-btn-social-size(1.3rem, 1.2rem, 1.1rem);
    .social-link-tooltip {
        margin-top: -5.5rem;
    }
}

a.social-link-size-3 {
    @include create-btn-social-size(1.4rem, 1.3rem, 1.2rem);
    .social-link-tooltip {
        margin-top: -6rem;
    }
}

a.social-link-size-4 {
    @include create-btn-social-size(1.8rem, 1.7rem, 1.6rem);
    .social-link-tooltip {
        margin-top: -7rem;
    }
}

a.social-link-color-dark {
    @include create-btn-social-color(
        $nav-contrast, lighten($nav-background, 10%), lighten($nav-background, 8%),
        $white, $primary, darken($primary, 3%)
    )
}

a.social-link-color-secondary {
    @include create-btn-social-color(
        $white, $dark, lighten($dark, 10%),
        $white, $primary, darken($primary, 5%)
    )
}

a.social-link-color-black {
    @include create-btn-social-color(
        $white, lighten($black, 7%), lighten($black, 13%),
        $white, $primary, darken($primary, 5%)
    )
}

a.social-link-color-light {
    @include create-btn-social-color(
        $white, lighten($dark, 5%), lighten($dark, 20%),
        $white, lighten($primary, 5%), lighten($primary, 20%),
    )
}

div.social-link-tooltip {
    position: absolute;
    text-transform: capitalize;
    background-color: $primary;
    color: $white;
    padding: 0.2rem 0.8rem;
    border-radius: 10px;
    display: none;

    &::before {
        content: "";
        position: absolute;
        top: 100%;
        left: 50%;
        margin-left: -5px;
        border-width: 5px;
        border-style: solid;
        border-color: $primary transparent transparent transparent;
    }
}

a.social-link:hover {
    .social-link-tooltip {
        @include media-breakpoint-up(md) {
            display: block!important;
        }
    }
}
.wechat-dialog{
    text-align: center;
    padding: 100px 50px;
    box-sizing:border-box
}
</style>