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

## Controler


defined('BASEPATH') or exit('No direct script access allowed');

use chriskacerguis\RestServer\RestController;

class C_api extends RestController
{

	function __construct()
	{
		parent::__construct();
	}

	function index_get()
	{

		$id = $this->get('id');

		if ($id === null) {
			$dt_perusahaan = $this->M_api->select('tb_perusahaan');
		} else {
			$dt_perusahaan = $this->M_api->select('tb_perusahaan', 'id_perusahaan', $id);
		}

		if ($dt_perusahaan) {
			$this->response([
				'status' => true,
				'data' => $dt_perusahaan
			], 200);
		} else {
			$this->response([
				'status' => false,
				'message' => 'Data tidak di temukan'
			], 404);
		}
	}

	function index_post()
	{

		$field = [
			'perusahaan_name' => $this->post('perusahaan_name'),
			'perusahaan_address' => $this->post('perusahaan_address'),
			'requirement' =>  $this->post('requirement')
		];

		$insert_prosess = $this->M_api->insert('tb_perusahaan', $field);

		if ($field) {
			$this->response([
				'status' => true,
				'message' => 'Data berhasil ditambahkan'
				// 'data' => $data
			], 200);
		}
	}

	function index_delete()
	{
		$id = $this->delete('id');
		if ($id === null) {
			$this->response([
				'status' => false,
				'message' => 'Mohon masukan id !'
			], 404);
		} else {
			if ($this->M_api->delete('tb_perusahaan', 'id_perusahaan', $id) > 0) {
				$this->response([
					'status' => true,
					'message' => 'Data berhasil dihapus !'
				], 201);
			} else {
				$this->response([
					'status' => false,
					'message' => 'Id tdak ditemukan !'
				], 404);
			}
		}
	}

	function index_put()
	{
		$id = $this->put('id');
		$field = array(
			'id_perusahaan' => $id,
			'perusahaan_name' => $this->put('perusahaan_name'),
			'perusahaan_address' => $this->put('perusahaan_address'),
			'requirement' => $this->put('requirement')
		);

		if ($id === null) {
			$this->response([
				'status' => false,
				'message' => 'Id tidak ditemukan !'
			], 404);
		} else {
			$perusahaan = $this->M_api->update('tb_perusahaan', $field, 'id_perusahaan', $id);
			if ($perusahaan) {
				$this->response([
					'status' => true,
					'message' => 'Data berhasil diperbaharui !'
				], 201);
			}
		}
	}
}

