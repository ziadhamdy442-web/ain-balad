# ain-balad
{
  "name": "reports-management-dashboard",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "^4.0.3"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": ["react-app", "react-app/jest"]
  },
  "browserslist": {
    "production": [">0.2%", "not dead", "not op_mini all"],
    "development": ["last 1 chrome version", "last 1 firefox version", "last 1 safari version"]
  }
}



import React, { useState } from 'react';
import './App.css';
import Sidebar from './components/Sidebar';
import ReportsManagement from './components/ReportsManagement';
import Dashboards from './components/Dashboards';
import AIReview from './components/AIReview';
import Analysis from './components/Analysis';
import Users from './components/Users';

function App() {
  const [currentPage, setCurrentPage] = useState('reports');
  const [user] = useState({
    name: 'Amr',
    role: 'Admin',
    avatar: 'https://i.pravatar.cc/150?img=1'
  });

  return (
    <div className="app-container">
      <Sidebar currentPage={currentPage} setCurrentPage={setCurrentPage} />
      <div className="main-content">
        <header className="app-header">
          <div className="header-title">
            <img src="https://img.icons8.com/color/96/000000/eye.png" alt="Logo" className="logo" />
          </div>
          <div className="header-right">
            <div className="user-profile">
              <img src={user.avatar} alt="User" className="user-avatar" />
              <div className="user-info">
                <p className="user-name">{user.name}</p>
                <p className="user-role">{user.role}</p>
              </div>
              <span className="dropdown-arrow">▼</span>
            </div>
          </div>
        </header>

        <main className="page-content">
          {currentPage === 'reports' && <ReportsManagement />}
          {currentPage === 'dashboards' && <Dashboards />}
          {currentPage === 'ai-review' && <AIReview />}
          {currentPage === 'analysis' && <Analysis />}
          {currentPage === 'users' && <Users />}
        </main>
      </div>
    </div>
  );
}
export default App;
import React from 'react';
import '../styles/Sidebar.css';

function Sidebar({ currentPage, setCurrentPage }) {
  const menuItems = [
    { id: 'dashboards', label: 'Dashboards', icon: '📊' },
    { id: 'reports', label: 'Reports', icon: '📋' },
    { id: 'ai-review', label: 'AI Review', icon: '🤖' },
    { id: 'analysis', label: 'Analysis', icon: '📈' },
    { id: 'users', label: 'Users', icon: '👥' }
  ];

  return (
    <aside className="sidebar">
      <div className="sidebar-logo">
        <div className="logo-circle">أم الـــــــــبلـــد</div>
      </div>

      <nav className="sidebar-nav">
        {menuItems.map((item) => (
          <button
            key={item.id}
            className={`nav-item ${currentPage === item.id ? 'active' : ''}`}
            onClick={() => setCurrentPage(item.id)}
          >
            <span className="icon">{item.icon}</span>
            <span className="label">{item.label}</span>
          </button>
        ))}
      </nav>

      <div className="sidebar-footer">
        <button className="logout-btn">🚪 Log Out</button>
      </div>
    </aside>
  );
}

export default Sidebar;


import React, { useState } from 'react';
import '../styles/ReportsManagement.css';
import FilterSection from './FilterSection';
import ReportsTable from './ReportsTable';

function ReportsManagement() {
  const [filters, setFilters] = useState({
    date: 'Last 30 Days',
    status: 'All',
    priority: 'All',
    department: 'All'
  });

  const [reports, setReports] = useState([
    {
      id: 1023,
      issue: 'Water leakage',
      department: 'Electricity',
      submittedDate: '12 oct 2025',
      priority: 'Low',
      status: 'New',
      actions: ['View', 'Delete']
    },
    {
      id: 1024,
      issue: 'Street light failure',
      department: 'Water',
      submittedDate: '11 Nov 2025',
      priority: 'Medium',
      status: 'Assigned',
      actions: ['View', 'Delete']
    },
    {
      id: 1025,
      issue: 'Garbage accumulation',
      department: 'Sewage',
      submittedDate: '22 April 2024',
      priority: 'High',
      status: 'Late',
      actions: ['View', 'Delete']
    },
    {
      id: 1026,
      issue: 'Road crack',
      department: 'Roads',
      submittedDate: '1 Mai 2024',
      priority: 'Medium',
      status: 'In Progress',
      actions: ['View', 'Delete']
    },
    {
      id: 1027,
      issue: 'Fire incident',
      department: 'Cleaning & Environment',
      submittedDate: '13 oct 2025',
      priority: 'High',
      status: 'Pending',
      actions: ['View', 'Delete']
    },
    {
      id: 1028,
      issue: 'Sewage overflow',
      department: 'Parks & Recreation',
      submittedDate: '5 oct 2025',
      priority: 'High',
      status: 'In Progress',
      actions: ['View', 'Delete']
    }
  ]);

  const handleFilterChange = (filterName, value) => {
    setFilters(prev => ({
      ...prev,
      [filterName]: value
    }));
  };

  const handleExportCSV = () => {
    const csv = [
      ['Report ID', 'Issue', 'Department', 'Submitted Date', 'Priority', 'Status'],
      ...reports.map(r => [r.id, r.issue, r.department, r.submittedDate, r.priority, r.status])
    ]
      .map(row => row.join(','))
      .join('\n');

    const element = document.createElement('a');
    element.setAttribute('href', `data:text/csv;charset=utf-8,${encodeURIComponent(csv)}`);
    element.setAttribute('download', 'reports.csv');
    element.style.display = 'none';
    document.body.appendChild(element);
    element.click();
    document.body.removeChild(element);
  };

  return (
    <div className="reports-management">
      <div className="header-section">
        <h1>Reports Management</h1>
        <button className="export-btn" onClick={handleExportCSV}>
          📥 Export CSV
        </button>
      </div>

      <FilterSection filters={filters} onFilterChange={handleFilterChange} />
      <ReportsTable reports={reports} setReports={setReports} />
    </div>
  );
}

export default ReportsManagement;


import React from 'react';
import '../styles/FilterSection.css';

function FilterSection({ filters, onFilterChange }) {
  return (
    <div className="filter-section">
      <div className="filter-group">
        <label>Date:</label>
        <select value={filters.date} onChange={(e) => onFilterChange('date', e.target.value)}>
          <option>Last 30 Days</option>
          <option>Last 7 Days</option>
          <option>Last 90 Days</option>
          <option>All Time</option>
        </select>
      </div>

      <div className="filter-group">
        <label>Status:</label>
        <select value={filters.status} onChange={(e) => onFilterChange('status', e.target.value)}>
          <option>All</option>
          <option>New</option>
          <option>Assigned</option>
          <option>In Progress</option>
          <option>Pending</option>
          <option>Late</option>
        </select>
      </div>

      <div className="filter-group">
        <label>Priority:</label>
        <select value={filters.priority} onChange={(e) => onFilterChange('priority', e.target.value)}>
          <option>All</option>
          <option>Low</option>
          <option>Medium</option>
          <option>High</option>
        </select>
      </div>

      <div className="filter-group">
        <label>Department:</label>
        <select value={filters.department} onChange={(e) => onFilterChange('department', e.target.value)}>
          <option>All</option>
          <option>Electricity</option>
          <option>Water</option>
          <option>Sewage</option>
          <option>Roads</option>
          <option>Cleaning & Environment</option>
          <option>Parks & Recreation</option>
        </select>
      </div>
    </div>
  );
}

export default FilterSection;

import React from 'react';
import '../styles/ReportsTable.css';

function ReportsTable({ reports, setReports }) {
  const getStatusClass = (status) => {
    return status.toLowerCase().replace(' ', '-');
  };

  const getPriorityClass = (priority) => {
    return priority.toLowerCase();
  };

  const handleDelete = (id) => {
    if (window.confirm('Are you sure you want to delete this report?')) {
      setReports(reports.filter(r => r.id !== id));
    }
  };

  return (
    <div className="reports-table-container">
      <table className="reports-table">
        <thead>
          <tr>
            <th>Report ID</th>
            <th>Issue</th>
            <th>Department</th>
            <th>Submitted Date</th>
            <th>Priority</th>
            <th>Status</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          {reports.map((report) => (
            <tr key={report.id}>
              <td className="report-id">#{report.id}</td>
              <td className="issue">{report.issue}</td>
              <td className="department">{report.department}</td>
              <td className="date">{report.submittedDate}</td>
              <td className={`priority ${getPriorityClass(report.priority)}`}>
                {report.priority}
              </td>
              <td>
                <span className={`status-badge ${getStatusClass(report.status)}`}>
                  {report.status}
                </span>
              </td>
              <td className="action-cell">
                <button className="view-btn">👁️ View</button>
                <button 
                  className="delete-btn"
                  onClick={() => handleDelete(report.id)}
                >
                  🗑️ Delete
                </button>
                <button className="close-btn">✕</button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default ReportsTable;
import React from 'react';

function Dashboards() {
  return (
    <div className="page">
      <h1>📊 Dashboards</h1>
      <p>Dashboard content will be displayed here.</p>
    </div>
  );
}

export default Dashboards;



import React from 'react';

function AIReview() {
  return (
    <div className="page">
      <h1>🤖 AI Review</h1>
      <p>AI Review content will be displayed here.</p>
    </div>
  );
}

export default AIReview;

import React from 'react';

function Analysis() {
  return (
    <div className="page">
      <h1>📈 Analysis</h1>
      <p>Analysis content will be displayed here.</p>
    </div>
  );
}

export default Analysis;







import React from 'react';

function Users() {
  return (
    <div className="page">
      <h1>👥 Users</h1>
      <p>Users content will be displayed here.</p>
    </div>
  );
}


export default Users;


* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f5f5;
}

.app-container {
  display: flex;
  height: 100vh;
  overflow: hidden;
}

.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  background-color: #f5f5f5;
}

.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 30px;
  background-color: white;
  border-bottom: 1px solid #e0e0e0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.header-title {
  font-size: 24px;
  font-weight: 600;
  color: #1976d2;
}

.logo {
  width: 50px;
  height: 50px;
}

.header-right {
  display: flex;
  align-items: center;
}

.user-profile {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  cursor: pointer;
}

.user-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  object-fit: cover;
}

.user-info {
  display: flex;
  flex-direction: column;
}

.user-name {
  font-size: 14px;
  font-weight: 600;
  color: #333;
}

.user-role {
  font-size: 12px;
  color: #999;
}

.dropdown-arrow {
  margin-left: 10px;
  color: #999;
}

.page-content {
  flex: 1;
  overflow-y: auto;
  padding: 30px;
}

.page {
  background-color: white;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}


.sidebar {
  width: 200px;
  background: linear-gradient(135deg, #1e5799 0%, #2c3e7f 100%);
  color: white;
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 20px 0;
  box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);
}

.sidebar-logo {
  padding: 20px;
  text-align: center;
  border-bottom: 2px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 20px;
}

.logo-circle {
  width: 80px;
  height: 80px;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  font-size: 12px;
  font-weight: bold;
  text-align: center;
  border: 3px solid rgba(255, 182, 193, 0.6);
}

.sidebar-nav {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 0 10px;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 15px;
  background-color: transparent;
  color: rgba(255, 255, 255, 0.7);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.nav-item:hover {
  background-color: rgba(255, 255, 255, 0.1);
  color: white;
}

.nav-item.active {
  background-color: #ffb84d;
  color: #1e5799;
  font-weight: 600;
}

.nav-item .icon {
  font-size: 18px;
}

.nav-item .label {
  flex: 1;
  text-align: left;
}

.sidebar-footer {
  padding: 20px 10px;
  border-top: 2px solid rgba(255, 255, 255, 0.1);
}

.logout-btn {
  width: 100%;
  padding: 12px 15px;
  background-color: rgba(255, 255, 255, 0.1);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.logout-btn:hover {
  background-color: rgba(255, 255, 255, 0.2);
}



.reports-management {
  background-color: white;
  border-radius: 8px;
  padding: 30px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.header-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
}

.header-section h1 {
  font-size: 28px;
  color: #1976d2;
  font-weight: 600;
}

.export-btn {
  padding: 12px 24px;
  background-color: #ffb84d;
  color: #333;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 8px;
}

.export-btn:hover {
  background-color: #ffa826;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(255, 184, 77, 0.3);
}

