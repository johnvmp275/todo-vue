<template>
    <div class="container-message">
        <div class="notification" v-for="(notification, index) in notifications" :key="index">
            <div class="states-task" :style="{ background: notification.color }">
                <span class="material-symbols-outlined">
                    {{ notification.icon }}
                </span>
            </div>
            <p>
                {{ notification.msg }}
            </p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        notifications: Array
    }
}
</script>

<style scoped>
.container-message {
    position: fixed;
    display: flex;
    flex-direction: column-reverse;
    top: 10px;
    right: 10px;
    align-items: end;
    width: 300px;
}

.notification {
    position: relative;
    width: 300px;
    height: 70px;
    display: flex;
    align-items: center;
    background: var(--background-white);
    border-radius: 5px;
    box-shadow: 0 0 10px 1px var(--background-black);
    margin-top: 20px;
    gap: 8px;
    z-index: 999999999;
    animation: NewNotification .8s;
}

.notification p {
    padding: 10px;
}

.states-task {
    padding: 10px;
    width: 60px;
    align-items: center;
    justify-content: center;
    display: flex;
    height: 100%;
    border-radius: 5px 0 0 5px;
}

.states-task span {
    color: var(--background-white);
}

@keyframes NewNotification {
    from {
        margin-top: -20px;
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
</style>