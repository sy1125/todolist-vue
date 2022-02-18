<template>
    <div class="modal modal__dim" @click.self="$emit('close')">
        <div class="modal__content">
            <p class="modal__text">
                <slot name="modal-text">(여기에 모달 내용이 들어가요)</slot>
            </p>
            <button class="modal__close" @click="$emit('close')">OK</button>
        </div>
    </div>
</template>