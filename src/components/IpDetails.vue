<template>
    <div class="details__container" v-if="!loading && !error">
        <div class="details__info details__info--ip">
            <h1>IP Address</h1>
            <p>{{geoLoc.ip}}</p>
        </div>
        <div class="details__info details__info--location">
            <h1>Location</h1>
            <p>{{geoLoc.location.city}}</p>
        </div>
        <div class="details__info details__info--time">
            <h1>Timezone</h1>
            <p>UTC {{geoLoc.location.timezone}}</p>
        </div>
        <div class="details__info details__info--isp">
            <h1>ISP</h1>
            <p>{{ geoLoc.isp }}</p>
            <!-- .isp.replace(/(\w)(\w*)/g,(g0,g1,g2) => g1.toUpperCase() + g2.toLowerCase()) -->
        </div>
    </div>
    <div class="loading-state details__container" v-if="loading && !error">
        <div class="details__info details__info--ip">
            <h1>IP Address</h1>
            <p></p>
        </div>
        <div class="details__info details__info--location">
            <h1>Location</h1>
            <p></p>
        </div>
        <div class="details__info details__info--time">
            <h1>Timezone</h1>
            <p></p>
        </div>
        <div class="details__info details__info--isp">
            <h1>ISP</h1>
            <p></p>
            <p></p>
        </div>
    </div>
    <div class="error-state details__container" v-if="error">
        <p class="error-state__main-text">Something went wrong!</p>
        <p class="error-state__sub-text">Adblockers might prevent this application to work properly</p>
    </div>
</template>

<script>
import { inject } from '@vue/runtime-core'
export default {
    setup() {
        const geoLoc = inject('geoLoc')
        const error = inject('error')
        const loading = inject('loading')
        
        return {
            geoLoc,
            loading,
            error
        }
    }
}
</script>

<style lang="scss">
.details__container {
    text-align: center;
    background-color: white;
    width: 88%;
    max-width: 69.38rem;
    border-radius: .875rem;
    padding: 1.63rem .75rem;
    box-shadow: 0px 3px 15px rgba(0, 0, 0, .18);
    position: absolute;
    z-index: 1000;
    top: 60%;
    top: clamp(11rem, 20%, 20rem);
    left: 50%;
    transform: translateX(-50%);

    @media (min-width: 530px) {
        display: flex;
        gap: 1em;
        align-content: space-between;
        justify-content: space-between;
        text-align: unset;
        top: 70%;
        min-height: 10rem;
        flex-flow: row wrap;
        
        padding: 2.25rem 2rem;
            .details__info {
                width: 48%;
                position: relative;
                &:nth-child(odd)::after {
                    content: '';
                    position: absolute;
                    right: -2%;
                    top: 50%;
                    transform: translateY(-50%);
                    background-color: var(--darkGray);
                    width: 1px;
                    height: 80%;
                }
            }
    }
    @media (min-width: 1000px) {
        flex-flow: row nowrap;
        height: unset;
        .details__info {
            width: 23%;
            &:not(:first-of-type) p{
                max-width: 13rem;
            }
            &:not(:last-child)::after {
                content: none;
                content: '';
                position: absolute;
                right: -2%;
                top: 50%;
                transform: translateY(-50%);
                background-color: var(--darkGray);
                width: 1px;
                height: 80%;
            }
        }
    }
    
    h1 {
        font-size: 0.8125rem;
        font-size: clamp(0.625rem, 7vh, 0.8125rem);
        font-weight: 700;
        letter-spacing: .75px;
        text-transform: uppercase;
        color: var(--darkGray)
        
    }
    p {
        font-size: clamp(1.25rem, 4vw, 1.75rem);
        font-weight: 500;
        padding-top: .4em;
    }
    .details__info + .details__info {
        margin-top: 1.63rem;
        @media (min-width: 530px) {
            margin-top: 0;
        }
    }
}
.loading-state.details__container {

    h1 {padding-bottom: 1.25em;}
    p {
        opacity: .10;
        background-color: var(--veryDarkGray);
        width: 100%;
        height: clamp(1.25rem, 4vw, 1.75rem);
        animation: subtleOpacityAnim 2s linear infinite;

        &:first-of-type {
            width: 90%
        }
        
        & + p {
            margin-top: .5rem;
            width: 70%;
        }
    }
}
.error-state {
    display: block;
    min-height: min-content !important; 
    .error-state__main-text {
        color: hsl(0, 97%, 61%);
        font-weight: 500;
        padding-top: 0;
    }
    .error-state__sub-text {
        opacity: .75;
        font-weight: 400;
        padding-top: .5rem;
        font-size: 1em;
    }
}

@keyframes subtleOpacityAnim {
    from{
        opacity: .20;
    }

    20% {
        opacity: .14;
    }
    60% {
        opacity: .10;
    }
    to {
        opacity: .20;
    }
}
</style>