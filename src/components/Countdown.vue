<template>
    <p>
        TODAY&nbsp;&nbsp;&nbsp;<span class="flipUnitContainer__futureDate">{{ formatDate(new Date()) }}</span>
    </p>
    <div class="flipUnitContainer">
        <div id="days" class="flipUnitContainer__box">
            <div class="flipUnitContainer__flip">
                <div class="upperCard">
                    <span>{{ days }}</span>
                </div>
                <div class="lowerCard">
                    <span>{{ formatNum(days) }}</span>
                </div>
                <div class="flipCard fold">
                    <span></span>
                </div>
                <div class="flipCard unfold">
                    <span></span>
                </div>
            </div>
            <p class="flipUnitContainer__txt">DAYS</p>
        </div>

        <div id="hours" class="flipUnitContainer__box">
            <div class="flipUnitContainer__flip">
                <div class="upperCard">
                    <span>{{ hours }}</span>
                </div>
                <div class="lowerCard">
                    <span>{{ formatNum(hours) }}</span>
                </div>
                <div class="flipCard fold">
                    <span></span>
                </div>
                <div class="flipCard unfold">
                    <span></span>
                </div>
            </div>
            <p class="flipUnitContainer__txt">HOURS</p>
        </div>

        <div id="minutes" class="flipUnitContainer__box">
            <div class="flipUnitContainer__flip">
                <div class="upperCard">
                    <span>{{ minutes }}</span>
                </div>
                <div class="lowerCard">
                    <span>{{ formatNum(minutes) }}</span>
                </div>
                <div class="flipCard fold">
                    <span></span>
                </div>
                <div class="flipCard unfold">
                    <span></span>
                </div>
            </div>
            <p class="flipUnitContainer__txt">MINUTES</p>
        </div>

        <div id="seconds" class="flipUnitContainer__box">
            <div class="flipUnitContainer__flip">
                <div class="upperCard">
                    <span>{{ seconds }}</span>
                </div>
                <div class="lowerCard">
                    <span>{{ formatNum(seconds) }}</span>
                </div>
                <div class="flipCard fold">
                    <span></span>
                </div>
                <div class="flipCard unfold">
                    <span></span>
                </div>
            </div>
            <p class="flipUnitContainer__txt">SECONDS</p>
        </div>
    </div>
    <p>
        UNTIL&nbsp;&nbsp;&nbsp;<span class="flipUnitContainer__futureDate">{{ formatDate(futureDate) }}</span>
    </p>
</template>

<script>
const el = {
    daysEl: 'days',
    hoursEl: 'hours',
    minutesEl: 'minutes',
    secondsEl: 'seconds',
};

export default {
    name: 'Timer',
    props: ['futureDate'],
    data() {
        return {
            diff: Date,
            timer: undefined,

            days: '',
            hours: '',
            minutes: '',
            seconds: '',
        };
    },
    methods: {
        getDiff() {
            let diffsec = this.diff - new Date().getTime();

            if (diffsec < 0) {
                clearInterval(this.timer);

                this.days = 'Ti';
                this.hours = 'me';
                this.minutes = "'s";
                this.seconds = 'up';

                return true;
            }

            const addZero = function(num) {
                return num < 10 ? '0' + num : num;
            };

            const d = Math.floor(diffsec / (1000 * 60 * 60 * 24));
            diffsec = diffsec % (1000 * 60 * 60 * 24);
            const h = Math.floor(diffsec / (1000 * 60 * 60));
            diffsec = diffsec % (1000 * 60 * 60);
            const m = Math.floor(diffsec / (1000 * 60));
            diffsec = diffsec % (1000 * 60);
            const s = Math.floor(diffsec / 1000);

            this.days = addZero(d);
            this.hours = addZero(h);
            this.minutes = addZero(m);
            this.seconds = addZero(s);
        },
        formatDate(date) {
            let d = new Date(date),
                month = (d.getMonth() + 1).toString(),
                day = d.getDate().toString(),
                year = d.getFullYear().toString();
            if (month.length < 2) month = '0' + month;
            if (day.length < 2) day = '0' + day;
            return [year, month, day].join('/');
        },
        formatNum(num) {
            let newNum = Number(num) + 1;
            if (isNaN(newNum)) {
                return '00';
            } else {
                newNum = newNum >= 60 ? 0 : newNum;
                return newNum < 10 ? '0' + newNum : newNum;
            }
        },
        flipAction(fl) {
            const self = this;
            const box = document.getElementById(fl);
            box.querySelectorAll('.flipCard').forEach(function(element) {
                if (element.classList.contains('fold')) {
                    element.classList.remove('fold');
                    element.classList.add('unfold');
                    element.getElementsByTagName('SPAN')[0].textContent = self[fl];
                } else if (element.classList.contains('unfold')) {
                    element.classList.remove('unfold');
                    element.classList.add('fold');
                }
            });
        },
    },
    mounted() {
        //console.log('component:', this.futureDate);
        this.diff = new Date(this.futureDate).getTime();
        this.getDiff();
        this.timer = setInterval(this.getDiff, 1000);
    },
    beforeUnmount() {
        clearInterval(this.timer);
    },
    watch: {
        days: function() {
            this.flipAction(el.daysEl);
        },
        hours: function() {
            this.flipAction(el.hoursEl);
        },
        minutes: function() {
            this.flipAction(el.minutesEl);
        },
        seconds: function() {
            this.flipAction(el.secondsEl);
        },
    },
};
</script>

<style scoped lang="scss">
@import '../assets/scss/main.scss';

.flipUnitContainer {
    display: flex;
    justify-content: center;
    width: 100%;

    @media only screen and (max-width: $bp-md) {
        flex-wrap: wrap;
    }

    &__box {
        display: flex;
        flex-direction: column;
        align-items: center;
        min-width: 140px;
        @media only screen and (max-width: $bp-md) {
            flex-basis: calc(50% - 30px);
            min-width: calc(50% - 30px);
        }

        &:not(:last-child) {
            margin-right: 30px;
            position: relative;

            &::after {
                content: ':';
                position: absolute;
                right: -20px;
                top: 30px;
                font-size: 40px;
            }
        }

        @media only screen and (max-width: $bp-md) {
            &:nth-child(even) {
                margin-right: 0;

                &::after {
                    display: none;
                }
            }
        }
    }

    &__flip {
        display: block;
        position: relative;
        width: 100%;
        height: 120px;
        perspective-origin: 50% 50%;
        perspective: 300px;
        background-color: white;
        border-radius: 3px;
        box-shadow: 0px 10px 10px -10px grey;
    }

    &__futureDate {
        font-size: 2rem;
        font-weight: 600;
        color: $color-primary;
    }
}

.upperCard,
.lowerCard {
    display: flex;
    position: relative;
    justify-content: center;
    width: 100%;
    height: 50%;
    overflow: hidden;
    border: 1px solid whitesmoke;
}
.upperCard span,
.lowerCard span,
.flipCard span {
    font-size: 10rem;
    color: $color-gray-dark-1;

    @media only screen and (max-width: $bp-md) {
        font-size: 9rem;
    }
}

.upperCard {
    align-items: flex-end;
    border-bottom: 0.5px solid whitesmoke;
    border-top-left-radius: 3px;
    border-top-right-radius: 3px;
}
.upperCard span {
    transform: translateY(50%);
}

.lowerCard {
    align-items: flex-start;
    border-top: 0.5px solid whitesmoke;
    border-bottom-left-radius: 3px;
    border-bottom-right-radius: 3px;
}
.lowerCard span {
    transform: translateY(-50%);
}

.flipCard {
    display: flex;
    justify-content: center;
    position: absolute;
    left: 0;
    width: 100%;
    height: 50%;
    overflow: hidden;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
}
.flipCard.unfold {
    top: 50%;
    align-items: flex-start;
    transform-origin: 50% 0%;
    transform: rotateX(180deg);
    background-color: white;
    border-bottom-left-radius: 3px;
    border-bottom-right-radius: 3px;
    border: 0.5px solid whitesmoke;
    border-top: 0.5px solid whitesmoke;
}
.flipCard.unfold span {
    transform: translateY(-50%);
}
.flipCard.fold {
    top: 0%;
    align-items: flex-end;
    transform-origin: 50% 100%;
    transform: rotateX(0deg);
    background-color: white;
    border-top-left-radius: 3px;
    border-top-right-radius: 3px;
    border: 0.5px solid whitesmoke;
    border-bottom: 0.5px solid whitesmoke;
}
.flipCard.fold span {
    transform: translateY(50%);
}

.fold {
    -webkit-animation: fold 0.6s cubic-bezier(0.455, 0.03, 0.515, 0.955) 0s 1 normal forwards;
    animation: fold 0.6s cubic-bezier(0.455, 0.03, 0.515, 0.955) 0s 1 normal forwards;
    transform-style: preserve-3d;
}

.unfold {
    -webkit-animation: unfold 0.6s cubic-bezier(0.455, 0.03, 0.515, 0.955) 0s 1 normal forwards;
    animation: unfold 0.6s cubic-bezier(0.455, 0.03, 0.515, 0.955) 0s 1 normal forwards;
    transform-style: preserve-3d;
}

@-webkit-keyframes fold {
    0% {
        transform: rotateX(0deg);
    }
    100% {
        transform: rotateX(-180deg);
    }
}

@keyframes fold {
    0% {
        transform: rotateX(0deg);
    }
    100% {
        transform: rotateX(-180deg);
    }
}
@-webkit-keyframes unfold {
    0% {
        transform: rotateX(180deg);
    }
    100% {
        transform: rotateX(0deg);
    }
}
@keyframes unfold {
    0% {
        transform: rotateX(180deg);
    }
    100% {
        transform: rotateX(0deg);
    }
}
</style>
