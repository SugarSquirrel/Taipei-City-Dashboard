<template>
    <footer class="app-footer">
        <div class="footer-content">
            <div class="footer-logo">
                <img src="/src/assets/images/TUIC.svg" alt="TUIC Logo" />
            </div>
            <div class="footer-info">
                <p>
                    Developed By Taipei Urban Intelligence Center 2023-2024
                </p>
                <p>
                    Department of Information Technology, Taipei City Government
                </p>
            </div>
            <div class="footer-team">
                <p>Lead Developer: Igor Ho (Full Stack Engineer)</p>
                <p>Data Pipelines: Iima Yu (Data Scientist)</p>
                <p>
                    Design and UX: Roy Lin (Fmr. Consultant), Chu Chen (Researcher)
                </p>
                <p>Systems: Ann Shih (Systems Engineer)</p>
                <p>
                    Testing: Jack Huang (Data Scientist), Ian Huang (Data Analysis Intern)
                </p>
            </div>
        </div>
    </footer>
</template>

<style>
.app-footer {
	border-top: 1px solid rgba(255, 255, 255, 0.2);
	display: flex;
	background-color: #1e1e1e;
	color: #ffffff;
	padding: 10px;
	font-size: 0.8rem;

	.footer-content {
		padding: 10px 0;
		max-width: 1200px;
		margin: 0 auto;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.footer-logo {
		padding: 0 15px;
		flex: 1;
		margin-bottom: 10px;
		img {
			filter: invert(1);
			height: 80px;
		}
	}

	.footer-info {
		padding: 0 15px;
		flex: 10;
		margin-top: 10px;
		text-align: right; /* 確保右側信息對齊 */
	}

	.footer-team {
		padding: 0 15px;
		flex: 10;
		margin-top: 10px;
		text-align: right; /* 確保右側信息對齊 */
	}

	p {
		margin: 5px 0;
		line-height: 10px;/* 字體間距 */
	}
}
</style>