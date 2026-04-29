# Mac Health Check 4
## [Inspect Mode](https://swiftdialog.app/advanced/inspect-mode/)

> The SwiftDialog Inspect Mode is a new built-in feature that enables real-time monitoring within the macOS filesystem. It tracks filesystem status (utilizing Apple’s FSEvents API) while monitoring application installations and inspecting cache folders, files, and plist content to visualize compliance checks. This feature is specifically designed for use during device enrollment, software deployment, and compliance auditing, providing end users with clear visibility into their compliance status.

While version `4.0.0` of Mac Health Check was _initially_ focused on **enterprise** reporting (by uploading `JSON` to a data warehouse), it dawned on me one morning that client-side `JSON` could be used to leverage Henry's sweet, sweet Inspect Mode for end-user reporting.

## Screenshots

<table>
	<tr>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.09%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.09%E2%80%AFAM.png" alt="Results Overview" width="320"></a>Results Overview</td>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.14%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.14%E2%80%AFAM.png" alt="Security Status" width="320"></a>Security Status</td>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.19%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.19%E2%80%AFAM.png" alt="Maintenance Status" width="320"></a>Maintenance Status</td>
	</tr>
	<tr>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.23%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.23%E2%80%AFAM.png" alt="MDM &amp; Inventory Status" width="320"></a>MDM &amp; Inventory Status</td>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.27%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.27%E2%80%AFAM.png" alt="Connectivity Status" width="320"></a>Connectivity Status</td>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.32%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.32%E2%80%AFAM.png" alt="Application Status" width="320"></a>Application Status</td>
	</tr>
	<tr>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.35%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.35%E2%80%AFAM.png" alt="Healthy Results" width="320"></a>Healthy Results</td>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.39%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.39%E2%80%AFAM.png" alt="Help &amp; Support" width="320"></a>Help &amp; Support</td>
		<td><a href="Screenshot%202026-04-29%20at%2011.15.46%E2%80%AFAM.png"><img src="Screenshot%202026-04-29%20at%2011.15.46%E2%80%AFAM.png" alt="Next Steps" width="320"></a>Next Steps</td>
	</tr>
</table>

