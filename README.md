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
export default App;
