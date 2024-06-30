<!-- Developed By Taipei Urban Intelligence Center 2023-2024 -->
<!--
Lead Developer:  Igor Ho (Full Stack Engineer)
Data Pipelines:  Iima Yu (Data Scientist)
Design and UX: Roy Lin (Fmr. Consultant), Chu Chen (Researcher)
Systems: Ann Shih (Systems Engineer)
Testing: Jack Huang (Data Scientist), Ian Huang (Data Analysis Intern)
-->
<!-- Department of Information Technology, Taipei City Government -->

<script setup>
// ⭐
import {
	onBeforeMount,
	onMounted,
	onBeforeUnmount,
	ref,
	computed,
	onUnmounted,
} from "vue";
import { useAuthStore } from "./store/authStore";
import { useDialogStore } from "./store/dialogStore";
import { useContentStore } from "./store/contentStore";

import NavBar from "./components/utilities/bars/NavBar.vue";
import SideBar from "./components/utilities/bars/SideBar.vue";
import AdminSideBar from "./components/utilities/bars/AdminSideBar.vue";
import SettingsBar from "./components/utilities/bars/SettingsBar.vue";
import NotificationBar from "./components/dialogs/NotificationBar.vue";
import InitialWarning from "./components/dialogs/InitialWarning.vue";
import ComponentSideBar from "./components/utilities/bars/ComponentSideBar.vue";
import LogIn from "./components/dialogs/LogIn.vue";

const authStore = useAuthStore();
const dialogStore = useDialogStore();
const contentStore = useContentStore();
const timeToUpdate = ref(600);
const showFooter = ref(false);

// ⭐
const checkFooterVisibility = () => {
	const scrollPosition = window.scrollY;
	const windowHeight = window.innerHeight;
	const fullHeight = document.documentElement.scrollHeight;
	const buffer = 50; // 增加缓冲区，使 footer 在接近底部时显示

	showFooter.value = scrollPosition + windowHeight >= fullHeight - buffer;
};

const formattedTimeToUpdate = computed(() => {
	const minutes = Math.floor(timeToUpdate.value / 60);
	const seconds = timeToUpdate.value % 60;
	return `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
});

function reloadChartData() {
	if (!["dashboard", "mapview"].includes(authStore.currentPath)) return;
	contentStore.setCurrentDashboardChartData();
	timeToUpdate.value = 600;
}
function updateTimeToUpdate() {
	if (!["dashboard", "mapview"].includes(authStore.currentPath)) return;
	if (timeToUpdate.value <= 0) {
		timeToUpdate.value = 0;
		return;
	}
	timeToUpdate.value -= 5;
}

onBeforeMount(() => {
	authStore.initialChecks();

	let vh = window.innerHeight * 0.01;
	document.documentElement.style.setProperty("--vh", `${vh}px`);

	window.addEventListener("resize", () => {
		let vh = window.innerHeight * 0.01;
		document.documentElement.style.setProperty("--vh", `${vh}px`);
	});
	// contentStore.wsConnect();
});
onMounted(() => {
	const showInitialWarning = localStorage.getItem("initialWarning");

	if (!showInitialWarning && !window.location.pathname.includes("embed")) {
		dialogStore.showDialog("initialWarning");
	}

	setInterval(reloadChartData, 1000 * 600);
	setInterval(updateTimeToUpdate, 1000 * 5);
});
onBeforeUnmount(() => {
	clearInterval(reloadChartData);
	clearInterval(updateTimeToUpdate);
	// contentStore.wsDisconnect();
});

onMounted(() => {
	window.addEventListener("scroll", checkFooterVisibility);
	checkFooterVisibility(); // 初始檢查
});

onUnmounted(() => {
	window.removeEventListener("scroll", checkFooterVisibility);
});
</script>

<template>
	<div class="app-container">
		<NotificationBar />
		<NavBar v-if="authStore.currentPath !== 'embed'" />
		<!-- /mapview, /dashboard layouts -->
		<div
			v-if="
				authStore.currentPath === 'mapview' ||
				authStore.currentPath === 'dashboard'
			"
			class="app-content"
		>
			<SideBar />
			<div class="app-content-main">
				<SettingsBar />
				<RouterView />
			</div>
		</div>
		<!-- /admin layouts -->
		<div v-else-if="authStore.currentPath === 'admin'" class="app-content">
			<AdminSideBar />
			<div class="app-content-main">
				<RouterView />
			</div>
		</div>
		<!-- /component, /component/:index layouts -->
		<div
			v-else-if="authStore.currentPath.includes('component')"
			class="app-content"
		>
			<ComponentSideBar />
			<div class="app-content-main">
				<RouterView />
			</div>
		</div>
		<div v-else>
			<router-view />
		</div>
		<InitialWarning />
		<LogIn />
		<div
			v-if="
				['dashboard', 'mapview'].includes(authStore.currentPath) &&
				!authStore.isMobile &&
				!authStore.isNarrowDevice
			"
			class="app-update"
		>
			<p>下次更新：{{ formattedTimeToUpdate }}</p>
		</div>
		<!-- ⭐在 app-container 的最後添加 footer -->
		<footer :class="['app-footer', { show: showFooter }]">
			<div class="footer-content">
				<div class="footer-logo">
					<img src="/src/assets/images/TUIC.svg" alt="TUIC Logo" />
				</div>
				<div class="footer-info">
					<p>
						Developed By Taipei Urban Intelligence Center 2023-2024
					</p>
					<p>
						Department of Information Technology, Taipei City
						Government
					</p>
				</div>
				<div class="footer-team">
					<p>Lead Developer: Igor Ho (Full Stack Engineer)</p>
					<p>Data Pipelines: Iima Yu (Data Scientist)</p>
					<p>
						Design and UX: Roy Lin (Fmr. Consultant), Chu Chen
						(Researcher)
					</p>
				</div>
			</div>
		</footer>
	</div>
</template>

<style scoped lang="scss">
.app {
	&-container {
		max-width: 100vw;
		max-height: 100vh;
		max-height: calc(var(--vh) * 100);
	}

	&-content {
		width: 100vw;
		max-width: 100vw;
		height: calc(100vh - 60px);
		height: calc(var(--vh) * 100 - 60px);
		display: flex;

		&-main {
			width: 100%;
			display: flex;
			flex-direction: column;
		}
	}

	&-update {
		position: fixed;
		bottom: 0;
		right: 20px;
		color: white;
		opacity: 0.3;
		transition: opacity 0.3s;
		user-select: none;

		p {
			color: var(--color-complement-text);
		}

		&:hover {
			opacity: 1;
		}
	}

	&-footer {
		position: fixed;
		bottom: -100%; // 初始时隐藏在视口下方
		left: 0;
		right: 0;
		transition: bottom 0.3s ease-in-out;
		background-color: #1e1e1e;
		color: #ffffff;
		padding: 20px;
		font-size: 0.8rem;
		text-align: center;
		z-index: 1000; // 確保 footer 在其他內容之上

		.footer-content {
			max-width: 1200px;
			margin: 0 auto;
			display: flex;
			flex-direction: column;
			align-items: center;
			text-align: center;
		}

		.footer-logo {
			margin-bottom: 10px;

			img {
				height: 40px;
			}
		}

		.footer-info,
		.footer-team {
			margin-top: 10px;
		}

		p {
			margin: 5px 0;
		}
	}

	&-footer.show {
		bottom: 0; // 显示时移动到底部
	}
}
</style>

<!-- <footer>
	<article class="footer_body">
		<div class="footer__hd">
			<div class="logo">
				<img src="/src/assets/images/TUIC.svg" />
			</div>
		</div>
		<div class="footer__bd">
			<div class="msgUnit">
				<div class="privacy">隱私權及安全政策</div>
			</div>
		</div>
		<div class="footer__ft"></div>
	</article>
</footer> -->
