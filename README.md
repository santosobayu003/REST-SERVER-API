# Library-chriskacerguis dan CodeIgniter 3
1. Install library didalam folder aplikasi pada project CodeIgniter 3.
2. Copy file Rest.php kedalam folder Aplikasi->Config pada project CodeIgniter 3.

## Model


class M_api extends CI_Model
{

    function select ($table, $where = null, $id = null)
    {
        if ($id === null) {
            return $this->db->get($table)->result_array();
        } else {
            return $this->db->get_where($table,  [$where => $id])->result_array();
        }
    }

    function insert($table, $field)
    {
        $this->db->insert($table, $field);
        return $this->db->affected_rows();
    }

    function delete($table, $where, $id)
    {
        $this->db->delete($table, [$where => $id]);
        return $this->db->affected_rows();
    }

    function update($table, $field, $where, $id)
    {
        $this->db->update($table, $field, [$where => $id]);
        return $this->db->affected_rows();
    }
}


