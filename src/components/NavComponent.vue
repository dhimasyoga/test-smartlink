<template>
    <nav>
        <i class="fa-solid fa-arrow-left prev-step text-blue hover-pointer"></i>
        <h5 v-text="navTitle" class="nav-title"></h5>
    </nav>
</template>

<style scoped>

nav {
    display: flex;
    align-items: center;
    justify-content: center;

    padding: 10px 15px;

    border-bottom: 2px solid #ddd;
    position: relative;
    font-weight: 500;
    width: 100%;
}

    h5 {
        margin: 0 auto;
    }

    .prev-step {
        position: absolute;
        left: 20px;

        top: 50%;
        transform: translateY(-50%);
    }

</style>

<script>
import jQuery from "jquery";

const $ = jQuery;
window.$ = $;

export default {
    name: 'NavComponent',
    props: {
        title: String
    },
    data: function(){
        return {
            navTitle: '',
        }
    },
    mounted() {
        if(this.title) {
            this.navTitle = this.title;
        }

        $('.prev-step').hide();
    }
}
</script>