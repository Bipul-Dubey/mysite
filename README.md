import Box from '@mui/material/Box';
import Table from '@mui/material/Table';
import TableBody from '@mui/material/TableBody';
import TableCell from '@mui/material/TableCell';
import TableContainer from '@mui/material/TableContainer';
import TableHead from '@mui/material/TableHead';
import TablePagination from '@mui/material/TablePagination';
import TableRow from '@mui/material/TableRow';
import Typography from '@mui/material/Typography';
import CircularProgress from '@mui/material/CircularProgress';


const DataTable = ({
  columns = [],
  rows = [],
  rowsPerPageOptions = [10, 25, 100],
  page = 0,
  rowsPerPage = 10,
  onPageChange = () => { },
  onRowsPerPageChange = () => { },
  onRowClick = () => { },
  pagination = false,
  emptyState = null,
  loading = false,
  tableHeaderColor = '#f5f5f5'
}) => {
  if (!Array.isArray(columns) || !Array.isArray(rows)) {
    console.error("columns and rows must be Array")
    return
  }
  return (
    <Box sx={{ width: '100%', overflow: 'hidden', border: "1px solid grey" }}>
      <TableContainer sx={{ height: `calc(100vh - 200px)` }}>
        <Table stickyHeader aria-label="custom table">
          <TableHead>
            <TableRow sx={{ backgroundColor: tableHeaderColor, }}>
              {columns.map((column) => (
                <TableCell
                  key={column.id}
                  align={column.align}
                  style={{ minWidth: column.minWidth, backgroundColor: tableHeaderColor, }}
                >
                  {column.label}
                </TableCell>
              ))}
            </TableRow>
          </TableHead>
          <TableBody>
            {loading ? (
              <TableRow>
                <TableCell colSpan={columns.length} align="center">
                  <CircularProgress />
                </TableCell>
              </TableRow>
            ) : rows.length === 0 ? (
              <TableRow>
                <TableCell colSpan={columns.length} align="center">
                  {emptyState ? emptyState : <Typography variant="body1">No data available</Typography>}

                </TableCell>
              </TableRow>
            ) : (
              rows.map((row, rowIndex) => (
                <TableRow hover role="checkbox" tabIndex={-1} key={rowIndex} onClick={() => onRowClick && onRowClick(row)}>
                  {columns.map((column) => {
                    const value = row[column.id];
                    return (
                      <TableCell key={column.id} align={column.align}>
                        {column.format && typeof value === 'number' ? column.format(value) : value}
                      </TableCell>
                    );
                  })}
                </TableRow>
              ))
            )}
          </TableBody>
        </Table>
      </TableContainer>
      {pagination && (
        <TablePagination
          rowsPerPageOptions={rowsPerPageOptions}
          component="div"
          count={rows.length}
          rowsPerPage={rowsPerPage}
          page={page}
          onPageChange={onPageChange}
          onRowsPerPageChange={onRowsPerPageChange}
        />
      )}
    </Box>
  );
};

export default DataTable;

