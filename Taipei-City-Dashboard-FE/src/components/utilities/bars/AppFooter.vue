


<template>
	<div class="footer-content">
	  <div class="footer-logo">
		<img src="/src/assets/images/TUIC.svg" alt="TUIC Logo" />
	  </div>
	  <div class="footer-info">
		<p>Developed By Taipei Urban Intelligence Center 2023-2024</p>
		<p>Department of Information Technology, Taipei City Government</p>
	  </div>
	  <div class="footer-team">
		<p>Lead Developer: Igor Ho (Full Stack Engineer)</p>
		<p>Data Pipelines: Iima Yu (Data Scientist)</p>
		<p>Design and UX: Roy Lin (Fmr. Consultant), Chu Chen (Researcher)</p>
		<p>Systems: Ann Shih (Systems Engineer)</p>
		<p>Testing: Jack Huang (Data Scientist), Ian Huang (Data Analysis Intern)</p>
	  </div>
	</div>
  </template>

  <style scoped>
  .footer-content {
	display: flex;
	justify-content: space-between;
	align-items: flex-start;
	padding: 20px;
	max-width: 1200px;
	margin: 0 auto;
	color: #ffffff;
	font-size: 0.8rem;
  }

  .footer-logo {
	flex: 1;
	display: flex;
	justify-content: flex-start;
	align-items: center;
  }

  .footer-logo img {
	filter: invert(1);
	height: 65px;
  }

  .footer-info, .footer-team {
	flex: 2;
	padding: 0 20px;
  }

  p {
	margin: 5px 0;
	line-height: 1.2;
  }

  @media (max-width: 768px) {
	.footer-content {
	  flex-direction: column;
	  align-items: center;
	}

	.footer-logo, .footer-info, .footer-team {
	  width: 100%;
	  padding: 10px 0;
	  text-align: center;
	}
  }
  </style>