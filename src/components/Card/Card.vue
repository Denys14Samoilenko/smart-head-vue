<script lang="ts">
import { User } from '../../types/User';

export default {
	props: {
		user: {
			type: Object as () => User,
			required: true,
		},
		isMenuOpen: {
			type: Boolean,
			required: true,
		},
	},
	data() {
		return {};
	},
	methods: {
		deleteUser() {
			this.$emit('delete-user', this.user.id);
		},

		showUserInfo() {
			this.$emit('show-user-info', this.user.id);
			this.$emit('is-modal-open', false);
		},
	},
	computed: {
		getUserEmailPath() {
			return 'mailto:' + this.user.email;
		},

		getFullName() {
			return `${this.user.first_name} ${this.user.last_name}`.trim();
		},
	},
};
</script>

<template>
	<div class="card user">
		<img class="user_avatar" :src="user.avatar" alt="user avatar" />

		<div class="user_content">
			<h2 class="user_name" @click="showUserInfo">{{ getFullName }}</h2>
			<a class="user_email" :href="getUserEmailPath">{{ user.email }}</a>
		</div>

		<button class="user_button" @click="deleteUser">X</button>
	</div>
</template>

<style scoped lang="scss">
.user {
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	align-items: center;
	gap: 10px;
	min-height: 100%;
	padding: 10px;

	@media (min-width: 640px) {
		flex-direction: row;
	}

	&_avatar {
		border-radius: 8px;
		width: 150px;
		object-fit: contain;
	}

	&_name {
		font-size: 32px;
		margin: 0;
		color: black ;
		transition: color 0.5s;
		word-break: break-all;

		&:hover {
			color: #646cff;
			cursor: pointer;
		}
	}

	&_email {
		font-size: 18px;
		color: black;
		transition: color 0.5s;

		&:hover {
			color: #0000ff;
		}
	}

	&_button {
		transition: all 0.5s;
		background-color: #fff;
		color: #000;

		&:hover {
			background-color: #646cff;
			color: #fff;
		}
	}
}
</style>
