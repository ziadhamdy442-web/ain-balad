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
export default App;
