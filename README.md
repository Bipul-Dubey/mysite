import React, { useState } from 'react';
import { useLocation, Link, useNavigate } from 'react-router-dom';
import {
  Drawer,
  List,
  ListItem,
  ListItemText,
  ListItemIcon,
  Collapse,
  IconButton,
  styled,
} from '@mui/material';
import {
  ChevronLeft,
  ChevronRight,
  ExpandLess,
  ExpandMore,
  Dashboard,
  People,
  Settings,
  Assessment,
  Business,
  Assignment,
  Notifications,
  Help,
  Logout,
} from '@mui/icons-material';

const DrawerWidth = 240;

const StyledDrawer = styled(Drawer)(({ theme, open }) => ({
  width: open ? DrawerWidth : 65,
  height: '100vh',
  flexShrink: 0,
  whiteSpace: 'nowrap',
  '& .MuiDrawer-paper': {
    width: open ? DrawerWidth : 65,
    transition: theme.transitions.create('width', {
      easing: theme.transitions.easing.sharp,
      duration: theme.transitions.duration.enteringScreen,
    }),
    overflowX: 'hidden',
    position: 'relative',
    display: 'flex',
    flexDirection: 'column',
    height: '100vh',
    '& ::-webkit-scrollbar': {
      width: '6px',
    },
    '& ::-webkit-scrollbar-track': {
      background: 'transparent',
    },
    '& ::-webkit-scrollbar-thumb': {
      background: 'rgba(0, 0, 0, 0.2)',
      borderRadius: '3px',
    },
    '& ::-webkit-scrollbar-thumb:hover': {
      background: 'rgba(0, 0, 0, 0.3)',
    },
    '& a': {
      color: 'inherit',
      textDecoration: 'none',
    },
  },
}));

const menuItems = [
  {
    text: 'Dashboard',
    icon: <Dashboard />,
    path: '/dashboard',
    children: [
      { text: 'Analytics', path: '/dashboard/analytics' },
      { text: 'Overview', path: '/dashboard/overview' },
      { text: 'Reports', path: '/dashboard/reports' },
    ],
  },
  {
    text: 'Analytics',
    icon: <Assessment />,
    path: '/analytics',
    children: [
      { text: 'Performance', path: '/analytics/performance' },
      { text: 'Metrics', path: '/analytics/metrics' },
      { text: 'Growth', path: '/analytics/growth' },
    ],
  },
  {
    text: 'Organization',
    icon: <Business />,
    path: '/organization',
    children: [
      { text: 'Departments', path: '/organization/departments' },
      { text: 'Teams', path: '/organization/teams' },
      { text: 'Roles', path: '/organization/roles' },
    ],
  },
  {
    text: 'Users',
    icon: <People />,
    path: '/users',
    children: [
      { text: 'User List', path: '/users/list' },
      { text: 'User Groups', path: '/users/groups' },
      { text: 'Permissions', path: '/users/permissions' },
    ],
  },
  {
    text: 'Tasks',
    icon: <Assignment />,
    path: '/tasks',
    children: [
      { text: 'My Tasks', path: '/tasks/my-tasks' },
      { text: 'Team Tasks', path: '/tasks/team' },
      { text: 'Archive', path: '/tasks/archive' },
    ],
  },
  {
    text: 'Notifications',
    icon: <Notifications />,
    path: '/notifications',
  },
  {
    text: 'Settings',
    icon: <Settings />,
    path: '/settings',
    children: [
      { text: 'General', path: '/settings/general' },
      { text: 'Security', path: '/settings/security' },
      { text: 'Preferences', path: '/settings/preferences' },
    ],
  },
  {
    text: 'Help',
    icon: <Help />,
    path: '/help',
  },
];

export default function LeftSidebar() {
  const [open, setOpen] = useState(true);
  const [expandedItems, setExpandedItems] = useState({});
  const location = useLocation();
  const navigate = useNavigate();

  const handleDrawerToggle = () => {
    setOpen(!open);
    if (!open) {
      setExpandedItems({}); // Reset expanded items when opening
    }
  };

  const handleItemClick = (item) => {
    if (!open) {
      // When drawer is closed, navigate to first child if available
      if (item.children && item.children.length > 0) {
        navigate(item.children[0].path);
      } else {
        navigate(item.path);
      }
      return;
    }

    if (item.children) {
      setExpandedItems((prev) => ({
        ...prev,
        [item.text]: !prev[item.text],
      }));
    }
  };

  const handleLogout = () => {
    console.log('Logging out...');
    // Implement your logout logic here
  };

  const isChildSelected = (item) => {
    return item.children?.some(child => location.pathname === child.path);
  };

  return (
    <>
      <IconButton
        onClick={handleDrawerToggle}
        sx={{
          position: 'fixed',
          left: open ? DrawerWidth - 28 : 37,
          top: 5,
          zIndex: 1201,
          transition: theme => theme.transitions.create('left', {
            easing: theme.transitions.easing.sharp,
            duration: theme.transitions.duration.enteringScreen,
          }),
          backgroundColor: 'white',
          '&:hover': {
            backgroundColor: 'rgba(0, 0, 0, 0.04)',
          },
          boxShadow: '0px 2px 4px rgba(0, 0, 0, 0.1)',
        }}
      >
        {open ? <ChevronLeft /> : <ChevronRight />}
      </IconButton>

      <StyledDrawer variant="permanent" open={open}>
        <List sx={{
          mt: 3,
          overflowY: 'auto',
          overflowX: 'hidden',
          flexGrow: 1
        }}>
          {menuItems.map((item) => (
            <React.Fragment key={item.text}>
              <ListItem
                button
                component={item.children && open ? 'div' : Link}
                to={item.children && open ? undefined : item.path}
                onClick={() => handleItemClick(item)}
                sx={{
                  minHeight: 48,
                  color: 'inherit',
                  backgroundColor: expandedItems[item.text]
                    ? 'rgba(0, 0, 0, 0.08)'
                    : location.pathname === item.path || isChildSelected(item)
                      ? 'rgba(0, 0, 0, 0.12)'
                      : 'transparent',
                  '&:hover': {
                    backgroundColor: expandedItems[item.text]
                      ? 'rgba(0, 0, 0, 0.12)'
                      : location.pathname === item.path || isChildSelected(item)
                        ? 'rgba(0, 0, 0, 0.16)'
                        : 'rgba(0, 0, 0, 0.04)',
                  },
                }}
              >
                <ListItemIcon sx={{ color: 'inherit' }}>{item.icon}</ListItemIcon>
                <ListItemText
                  primary={item.text}
                  sx={{
                    opacity: open ? 1 : 0,
                    whiteSpace: 'nowrap',
                    color: 'inherit',
                  }}
                />
                {open && item.children && (
                  expandedItems[item.text] ? <ExpandLess /> : <ExpandMore />
                )}
              </ListItem>

              {open && item.children && (
                <Collapse in={expandedItems[item.text]} timeout="auto" unmountOnExit>
                  <List component="div" disablePadding>
                    {item.children.map((child) => (
                      <ListItem
                        button
                        component={Link}
                        to={child.path}
                        key={child.text}
                        sx={{
                          pl: 4,
                          minHeight: 48,
                          color: 'inherit',
                          backgroundColor: location.pathname === child.path
                            ? 'rgba(0, 0, 0, 0.12)'
                            : 'rgba(0, 0, 0, 0.04)',
                          '&:hover': {
                            backgroundColor: location.pathname === child.path
                              ? 'rgba(0, 0, 0, 0.16)'
                              : 'rgba(0, 0, 0, 0.08)',
                          },
                        }}
                      >
                        <ListItemText
                          primary={child.text}
                          sx={{
                            whiteSpace: 'nowrap',
                            color: 'inherit',
                          }}
                        />
                      </ListItem>
                    ))}
                  </List>
                </Collapse>
              )}
            </React.Fragment>
          ))}
        </List>

        <List sx={{
          borderTop: '1px solid rgba(0, 0, 0, 0.12)',
          marginTop: 'auto',
        }}>
          <ListItem
            button
            onClick={handleLogout}
            sx={{
              minHeight: 48,
              color: 'inherit',
              '&:hover': {
                backgroundColor: 'rgba(0, 0, 0, 0.04)',
              },
            }}
          >
            <ListItemIcon sx={{ color: 'inherit' }}>
              <Logout />
            </ListItemIcon>
            <ListItemText
              primary="Logout"
              sx={{
                opacity: open ? 1 : 0,
                whiteSpace: 'nowrap',
                color: 'inherit',
              }}
            />
          </ListItem>
        </List>
      </StyledDrawer>
    </>
  );
}
