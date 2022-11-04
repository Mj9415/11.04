<template>
    <i-dialog
        id="style_test"
        :visible.sync="showDialog"
        :content-style="{ width: '16vw' }"
        :modal="false"
        :draggable="true"
    >
        <template #header> 계정 패스워드 변경 </template>

        <div class="p-fluid p-input-filled">
            <div class="p-field">
                <label for="password">새로운 패스워드</label>
                <Password
                    v-model="newUserPassword"
                    :feedback="false"
                    :class="{
                        'p-invalid': invalidMessageOnCode.NEWPASSWORD
                    }"
                    @input="onInputnewUserPassword"
                />
                <small class="p-error">
                    {{ invalidMessageOnCode.NEWPASSWORD }}
                </small>
            </div>

            <div class="p-field">
                <label for="password-reconfirm">패스워드 확인</label>
                <Password
                    v-model="userPasswordReconfirm"
                    :feedback="false"
                    :class="{
                        'p-invalid': invalidMessageOnCode.RECONFIRM
                    }"
                    @input="onInputuserPasswordReconfirm"
                />
                <small class="p-error">
                    {{ invalidMessageOnCode.RECONFIRM }}
                </small>
            </div>
        </div>

        <template #footer>
            <div class="p-fluid">
                <Button
                    label="적용"
                    icon="pi pi-check"
                    :style="{ width: '100%' }"
                    :disabled="applyDisabled"
                    @click="onCilckAppliedButton"
                />
            </div>
        </template>
    </i-dialog>
</template>

<script lang="ts">
import Vue from 'vue';
import Component from '@/plugins/nuxt-class-component';

@Component<UserPasswordSettingPanel>({
    props: {
        visible: Boolean
    }
})
export default class UserPasswordSettingPanel extends Vue {
    newUserPassword: string = '';
    userPasswordReconfirm: string = '';

    invalidMessageOnCode = {
        NEWPASSWORD: undefined as string | undefined,
        RECONFIRM: undefined as string | undefined
    };

    onInputnewUserPassword(input: string) {
        // by Mj 2022.11.04: 함수명(파라미터명:이벤트타입)
        const _input = input.toString(); // by Mj 2022.11.04: 값이 숫자일시 문자로 변환할 수 있게 toString
        if (_input.length > 8) {
            this.invalidMessageOnCode.NEWPASSWORD = '※ 8자 이하 입력해주세요';
        } else {
            this.invalidMessageOnCode.NEWPASSWORD = undefined;
        }
    }

    onInputuserPasswordReconfirm(input: string) {
        const _input = input.toString();
        if (this.newUserPassword != this.userPasswordReconfirm) {
            this.invalidMessageOnCode.RECONFIRM =
                '※ 동일한 패스워드를 입력해주세요';
        } else {
            this.invalidMessageOnCode.RECONFIRM = undefined;
        }
    }

    onCilckAppliedButton() {
        // by Mj 2022.11.04: 적용버튼 부분
    }

    get applyDisabled(): boolean {
        // by Mj 2022.11.04: 이부분을 고치기?
        // by Mj 2022.11.04: 버튼 비활성화 부분
        return (
            // by Mj 2022.11.04: 패스워드 둘다 다를때 버튼 비활성화
            this.newUserPassword != this.userPasswordReconfirm ||
            this.newUserPassword == '' ||
            this.userPasswordReconfirm == ''
        );
    }

    get showDialog(): boolean {
        return this.$props.visible;
    }

    set showDialog(_is_show: boolean) {
        this.$emit('update:visible', _is_show);
    }
}
</script>
<style lang="scss" scoped>
#style_test::v-deep {
    .p-dialog .p-dialog-header {
        font-size: 14px !important;
        font-weight: bolder !important;
    }
}
</style>
